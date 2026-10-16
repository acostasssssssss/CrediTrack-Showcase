# Arquitectura de la Plataforma CrediTrack SaaS

Diagrama general de comunicación y diseño de capas de la plataforma **CrediTrack**, destacando la arquitectura *Offline-First* en el cliente móvil, el panel administrativo web y los servicios backend contenerizados en la nube.

```mermaid
flowchart TD
    %% ─────────────────────────────────────────────────────────────
    %% CAPA CLIENTE (FRONTEND & MÓVIL)
    %% ─────────────────────────────────────────────────────────────
    subgraph Frontend["Capa Cliente (Frontend & Mobile)"]
        direction TB

        subgraph WebAdmin["Web Admin Dashboard (React + Tailwind)"]
            direction TB
            WebUI["UI Dashboard & Panel Operativo<br/><b>React 19 + Vite + Tailwind CSS</b>"]
            WebState["Context & React Router<br/><b>Gestión de Estado & Vistas</b>"]
            WebUI <--> WebState
        end

        subgraph MobileApp["App Móvil Cobrador (Flutter - Offline First)"]
            direction TB
            MobileUI["Interfaz de Usuario Móvil<br/><b>Flutter (Clean Architecture)</b>"]
            Riverpod["Gestor de Estado Reactivo<br/><b>Riverpod Providers</b>"]
            DriftDB[("Base de Datos Local Offline<br/><b>Drift / SQLite</b>")]
            SecureStorage["Almacén Cifrado<br/><b>Flutter Secure Storage</b>"]

            MobileUI <--> Riverpod
            Riverpod <--> DriftDB
            Riverpod <--> SecureStorage
        end
    end

    %% ─────────────────────────────────────────────────────────────
    %% CAPA DE RED / COMUNICACIÓN
    %% ─────────────────────────────────────────────────────────────
    WebAdmin <== "API REST (HTTPS / JSON)<br/>Tiempo Real" ==> APIServer
    MobileApp <== "API REST (HTTPS / JSON)<br/><b>Background Sync (Recuperación de red)</b>" ==> APIServer

    %% ─────────────────────────────────────────────────────────────
    %% CAPA DE BACKEND & NUBE
    %% ─────────────────────────────────────────────────────────────
    subgraph Backend["Capa de Backend (Nube / Cloud Infrastructure)"]
        direction TB

        subgraph DockerContainer["Entorno Contenerizado (Docker)"]
            APIServer["Servidor API Central<br/><b>Python / FastAPI + SQLAlchemy</b>"]
            Engine["Motor Financiero & Sync Engine<br/><b>Validación de Cuotas y Cierres</b>"]
            APIServer <--> Engine
        end

        CentralDB[("Base de Datos Central<br/><b>PostgreSQL (Cloud SaaS)</b>")]
        APIServer <== "Pool de Conexiones (Asyncpg)" ==> CentralDB
    end

    %% ─────────────────────────────────────────────────────────────
    %% ESTILOS VISUALES (CLASSDEF)
    %% ─────────────────────────────────────────────────────────────
    classDef webStyle fill:#1E293B,stroke:#38BDF8,stroke-width:2px,color:#F8FAFC
    classDef mobileStyle fill:#0F172A,stroke:#10B981,stroke-width:2px,color:#F8FAFC
    classDef dbStyle fill:#064E3B,stroke:#34D399,stroke-width:2px,color:#ECFDF5
    classDef securityStyle fill:#701A75,stroke:#E879F9,stroke-width:1.5px,color:#FDF4FF
    classDef backendStyle fill:#18181B,stroke:#F97316,stroke-width:2px,color:#FAFAFA
    classDef cloudDbStyle fill:#431407,stroke:#FB923C,stroke-width:2px,color:#FFF7ED
    classDef containerGroup fill:#0B131B,stroke:#334155,stroke-width:1.5px,color:#94A3B8

    class WebUI,WebState webStyle
    class MobileUI,Riverpod mobileStyle
    class DriftDB dbStyle
    class SecureStorage securityStyle
    class APIServer,Engine backendStyle
    class CentralDB cloudDbStyle
    class Frontend,Backend,DockerContainer,WebAdmin,MobileApp containerGroup
```
