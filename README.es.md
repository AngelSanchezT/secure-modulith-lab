# 🔐 Spring-Angular-Keycloak Lab

### Un laboratorio de integración Full-Stack para arquitecturas seguras.

![Angular](https://img.shields.io/badge/Angular-20-DD0031?logo=angular&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.x-6DB33F?logo=springboot&logoColor=white)
![Java](https://img.shields.io/badge/Java-25-007396?logo=openjdk&logoColor=white)
![Keycloak](https://img.shields.io/badge/Keycloak-Latest-4D4D4D?logo=keycloak&logoColor=white)

![OAuth2](https://img.shields.io/badge/OAuth2-Authorization-orange)
![OpenID Connect](https://img.shields.io/badge/OpenID%20Connect-OIDC-blue)
![Architecture](https://img.shields.io/badge/Architecture-Modular%20Monolith-blueviolet)

Este repositorio es un proyecto de demostración ("Proof of Concept") diseñado para explorar y dominar la integración de una arquitectura Monolito Modular moderna, utilizando Spring Boot para el backend, Angular para el frontend y Keycloak como proveedor de identidad centralizado (IdP).

🚫 Disclaimer: Este es un proyecto personal con fines educativos y de investigación. No contiene lógica de negocio propietaria ni datos reales.

## 🎯 Objetivos del Proyecto

El propósito principal de este laboratorio es resolver los desafíos técnicos comunes en sistemas distribuidos y seguros:

1. Desacoplamiento de Identidad: Delegar toda la autenticación y gestión de usuarios a Keycloak (OIDC/OAuth2).

2. Seguridad en Monolito Modular: Implementar seguridad a nivel de métodos y endpoints en Spring Boot basada en Roles (RBAC), manteniendo la independencia de los módulos.

3. Experiencia de Usuario Transparente: Integrar Angular con Keycloak para manejar tokens, refrescos de sesión y rutas protegidas (Guards) sin fricción para el usuario.

4. Orquestación Local: Configurar un entorno de desarrollo reproducible usando Docker.

## 🛠️ Stack Tecnológico

### Backend (The Modulith)

- Java 17/21: Lenguaje base.

- Spring Boot 3.x: Framework principal.

- Spring Security 6: Gestión de seguridad (Resource Server).

- Spring Data JPA: Persistencia.

- Estructura: Monolito Modular (paquetes feature-based o domain-driven).

### Frontend (SPA)

- Angular 16+: Framework de UI.

- Angular OAuth2 OIDC: Librería para manejo de flujos OpenID Connect.

- TailwindCSS: Estilizado rápido.

### Infraestructura & Identidad

- Keycloak (Docker): Servidor de Identidad y Gestión de Acceso.

- PostgreSQL: Base de datos relacional.

- Docker Compose: Orquestación de contenedores.

## 🏗️ Arquitectura de Alto Nivel

El sistema sigue un patrón de Resource Server. El Frontend no envía credenciales al Backend; envía un Access Token (JWT) firmado por Keycloak.

```mermaid

graph TD
    User((Usuario))
    
    subgraph "Cliente (Navegador)"
        SPA[Angular SPA]
    end
    
    subgraph "Infraestructura (Docker)"
        KC[Keycloak IdP]
        DB[(PostgreSQL)]
    end
    
    subgraph "Servidor de Aplicación"
        API[Spring Boot Modulith]
        Mod1[Módulo: Core/Usuarios]
        Mod2[Módulo: Ventas/Demo]
    end

    User -->|1. Login| SPA
    SPA -->|2. Redirección Auth| KC
    KC -->|3. Retorna Token JWT| SPA
    SPA -->|4. API Request + Bearer Token| API
    API -->|5. Validar Firma Token (JWK)| KC
    API -->|6. Persistencia| DB
    
    API --- Mod1
    API --- Mod2
    
```