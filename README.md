# 🔐 Spring-Angular-Keycloak Lab

### A Full-Stack Integration Lab for Secure Architectures (AuthN/AuthZ with Keycloak).

![Angular](https://img.shields.io/badge/Angular-20-DD0031?logo=angular&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.x-6DB33F?logo=springboot&logoColor=white)
![Java](https://img.shields.io/badge/Java-25-007396?logo=openjdk&logoColor=white)
![Keycloak](https://img.shields.io/badge/Keycloak-Latest-4D4D4D?logo=keycloak&logoColor=white)

![OAuth2](https://img.shields.io/badge/OAuth2-Authorization-orange)
![OpenID Connect](https://img.shields.io/badge/OpenID%20Connect-OIDC-blue)
![Architecture](https://img.shields.io/badge/Architecture-Modular%20Monolith-blueviolet)


This repository is a Proof of Concept (PoC) designed to explore and master the integration of a modern Modular Monolith architecture, using Spring Boot for the backend, Angular for the frontend, and Keycloak as the centralized Identity Provider (IdP).

🚫 Disclaimer: This is a personal project for educational and research purposes. It contains no proprietary business logic or real data.

## 🎯 Project Objectives

The main purpose of this lab is to solve common technical challenges in distributed and secure systems:

1. Identity Decoupling: Delegate all authentication and user management to Keycloak (OIDC/OAuth2).

2. Security in Modular Monolith: Implement RBAC (Role-Based Access Control) security at the method and endpoint level in Spring Boot, while maintaining module independence.

3. Transparent User Experience: Integrate Angular with Keycloak to handle tokens, session refreshes, and protected routes (Guards) without friction for the user.

4. Local Orchestration: Configure a reproducible development environment using Docker.

## 🛠️ Tech Stack

### Backend (The Modulith)

- Java 21: Base language.

- Spring Boot 3.5.x: Main framework.

- Spring Security 6: Security management (Resource Server).

- Spring Data JPA: Persistence.

- Structure: Modular Monolith (feature-based or domain-driven packages).

### Frontend (SPA)

- Angular 20+: UI Framework.

- Angular OAuth2 OIDC: Library for handling OpenID Connect flows.

- TailwindCSS: Rapid styling.

### Infrastructure & Identity

- Keycloak (Docker): Identity and Access Management Server.

- PostgreSQL: Relational Database.

- Docker Compose: Container orchestration.

## 🏗️ High-Level Architecture

The system follows a Resource Server pattern. The Frontend does not send credentials to the Backend; it sends an Access Token (JWT) signed by Keycloak.

```mermaid

graph TD
    User((User))
    
    subgraph "Client (Browser)"
        SPA[Angular SPA]
    end
    
    subgraph "Infrastructure (Docker)"
        KC[Keycloak IdP]
        DB[(PostgreSQL)]
    end
    
    subgraph "Application Server"
        API[Spring Boot Modulith]
        Mod1[Module: Core/Users]
        Mod2[Module: Sales/Demo]
    end

    User -->|1. Login| SPA
    SPA -->|2. Auth Redirection| KC
    KC -->|3. Returns JWT Token| SPA
    SPA -->|4. API Request + Bearer Token| API
    API -->|5. Validate Token Signature JWK| KC
    API -->|6. Persistence| DB
    
    API --- Mod1
    API --- Mod2

```