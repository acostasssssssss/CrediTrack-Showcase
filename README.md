# CrediTrack SaaS - Plataforma Fintech de Recaudo en Terreno

<div align="center">
  <img src="assets/mockups/inicio.png" width="65%" alt="Dashboard Web" />
  <img src="assets/mockups/off-line.jpg" width="30%" alt="App Móvil Offline" />
</div>

<br />

**CrediTrack** es una plataforma SaaS B2B integral y de misión crítica diseñada específicamente para microfinancieras, empresas de crédito independiente y equipos de cobranza de campo. El sistema digitaliza y automatiza el ciclo operativo completo: administración de cartera, liquidación de cuotas, trazabilidad de cobradores en ruta y sincronización de pagos bajo condiciones de red nula o intermitente (*Offline-First*).

> **Nota sobre el código fuente:** Este repositorio constituye una exhibición técnica y arquitectónica (*Showcase*) con fines demostrativos y de portafolio. El código fuente completo, esquemas internos de base de datos y algoritmos de balance financiero se mantienen en repositorios privados bajo estrictos acuerdos de propiedad intelectual y confidencialidad comercial.

---

## 💻 1. Ecosistema Web: Panel Administrativo (React + Tailwind)

El panel de administración para propietarios y auditores fue concebido bajo una arquitectura moderna **Mobile-First** con un sistema de diseño estructurado en **Bento Grid**. Esta disposición garantiza alta densidad de información financiera en tiempo real, adaptabilidad responsiva absoluta y dashboards analíticos claros sin saturación visual.

### Métricas y Reportes
Permite el monitoreo dinámico del flujo de caja, balance de préstamos activos, mora histórica, gastos consolidados del día y proyecciones de recaudación con filtros multidimensionales por ruta o cobrador.

<div align="center">
  <img src="assets/mockups/Reportes.png" width="49%" alt="Métricas" />
  <img src="assets/mockups/reportes2.png" width="49%" alt="Analítica Detallada" />
</div>

### Gestión de Cartera y Rutas
Módulo centralizado para el alta de prestatarios, asignación y auditoría de rutas de cobro, calificación de riesgo, seguimiento de microcréditos activos y visualización del recaudo esperado para la jornada.

<div align="center">
  <img src="assets/mockups/clientes.png" width="49%" alt="Lista de Clientes" />
  <img src="assets/mockups/cobros_de _hoy.png" width="49%" alt="Cobros de Hoy" />
</div>

### Detalle de Cliente (Vista Administrador)
Vista de control interno financiero diseñada para auditar exhaustivamente el historial de pagos, balance de cuotas pendientes, días sin abono y el estado general de la deuda de un prestatario en específico.

<div align="center">
  <img src="assets/mockups/cliente_cartulina.png" width="80%" alt="Detalle de Cliente Vista Administrador" />
</div>

### Recibo Digital Público (B2B2C)
Comprobante digital interactivo accesible mediante enlaces tokenizados seguros (*Magic Links*) que se despachan automáticamente al cliente final por WhatsApp. Permite consultar el estado de cuenta actualizado de forma transparente sin requerir credenciales de acceso.

<div align="center">
  <img src="assets/mockups/recibo_digital.jpg" width="40%" alt="Recibo Digital Público" />
</div>

### Configuración de Negocio
Centro de control administrativo con soporte multi-rol (Dueño / Cobrador). Permite parametrizar tasas de interés por defecto, modalidades de liquidación diaria/semanal, días festivos y umbrales de alerta de caja.

<div align="center">
  <img src="assets/mockups/configuracion.png" width="80%" alt="Configuración de Negocio" />
</div>

---

## 📱 2. Ecosistema Móvil: App para Cobradores (Flutter + Drift)

Herramienta nativa para operadores en terreno diseñada con una arquitectura de resiliencia extrema **Offline-First**. Garantiza que el cobrador registre transacciones, consulte saldos y realice cálculos financieros instantáneos incluso en zonas rurales sin cobertura móvil.

### 🎥 Experiencia de Usuario (Motion UI & Glassmorphism)
La aplicación implementa microinteracciones fluidas a 60 FPS, navegación háptica con barra flotante desacoplada y estética *Glassmorphism* con desenfoque de fondo en tiempo real para optimizar la ergonomía y velocidad de captura en calle.

<div align="center">
  <video src="assets/videos/inico.mp4" width="35%" controls autoplay loop muted></video>
  <video src="assets/videos/general.mp4" width="35%" controls autoplay loop muted></video>
</div>

### Operación en Terreno (Offline)
La app persiste cada abono y gasto localmente mediante **Drift / SQLite**, calculando los balances en el dispositivo en microsegundos. Al reestablecer conectividad celular o Wi-Fi, el motor de sincronización procesa las colas pendientes contra el servidor central sin interrupciones ni pérdida de datos.

<div align="center">
  <img src="assets/mockups/off-line.jpg" width="35%" alt="Modo Offline" />
</div>

### Seguridad, Accesos y Cifrado
Persistencia de sesiones de alta seguridad mediante tokens cifrados en hardware con **Flutter Secure Storage** (Keystore en Android / Keychain en iOS). La interfaz proporciona módulos dedicados para autenticación biométrica, reseteo de claves con tokens de un solo uso y políticas de revocación remota de dispositivos.

<div align="center">
  <img src="assets/mockups/contraseñ.jpg" width="35%" alt="Seguridad" />
  <img src="assets/mockups/conf.jpg" width="35%" alt="Configuración App" />
</div>

---

## 🛠️ 3. Arquitectura y Stack Tecnológico

* **Frontend Web:** React 19, Vite, Tailwind CSS, Lucide Icons, Framer Motion.
* **Aplicación Móvil:** Flutter (Clean Architecture), Riverpod (State Management), Drift / SQLite (Base de datos local), Flutter Secure Storage.
* **Backend & Servicios:** Python (FastAPI / SQLAlchemy), Pydantic, Uvicorn, Docker & Docker Compose.
* **Bases de Datos & Almacenamiento:** PostgreSQL en la nube (instancia SaaS centralizada) y SQLite embebido en dispositivos cliente.
* **Seguridad & Protocolos:** JWT con expiración estricta, Hash bcrypt, Cifrado TLS/HTTPS, esquemas Zero-Trust para enlaces públicos.

### Diagrama de Flujo y Comunicación

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

---

## 🧠 4. Retos de Ingeniería Resueltos

1. **Resolución de Conflictos Offline y Colas Idempotentes:**
   Implementación de un motor de sincronización bidireccional con marcas de tiempo lógicas y hashes de transacción. Cuando múltiples abonos se registran sin conexión en zonas remotas, el backend procesa las colas asegurando idempotencia estricta, conciliando balances sin duplicaciones ni inconsistencias contables.

2. **Prevención de Layout Blowouts en Motores Móviles Restrictivos:**
   Optimización del motor de renderizado CSS/Tailwind para mitigar fallos de desbordamiento horizontal (*viewport blowout*) en navegadores in-app y navegadores propietarios (ej. WebView de Huawei). Se implementaron contenedores estrictos con límites `overflow-x-hidden`, empaquetado de texto forzado (`break-words`) y dimensionamiento elástico no destructivo en componentes gráficos.

3. **Seguridad de Sesiones en Cold Start:**
   Garantía de inicialización atómica de credenciales en arranques en frío de la aplicación móvil. El pipeline de inicio verifica la integridad del almacenamiento seguro local antes de montar el árbol de widgets de Riverpod, evitando parpadeos de redirección no autenticada y asegurando persistencia transparente en modo avión.

---

## 📄 Licencia

Este es un proyecto comercial de código cerrado. Todos los derechos reservados © 2026 Carlos Acosta. Queda prohibida la reproducción total o parcial del diseño, material multimedia y la arquitectura aquí expuesta sin autorización expresa.
