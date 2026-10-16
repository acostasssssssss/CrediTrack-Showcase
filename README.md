# CrediTrack SaaS - Plataforma Fintech de Recaudo en Terreno

<div align="center">
  <img src="assets/mockups/inicio.png" width="65%" alt="Dashboard Web" />
</div>

<br />

**CrediTrack** es una plataforma SaaS B2B integral y de misión crítica diseñada específicamente para microfinancieras, empresas de crédito independiente y equipos de cobranza de campo. El sistema digitaliza y automatiza el ciclo operativo completo: administración de cartera, liquidación de cuotas, trazabilidad de cobradores en ruta y sincronización de pagos bajo condiciones de red nula o intermitente (*Offline-First*).

> **Nota sobre el código fuente y privacidad de datos:** Este repositorio constituye una exhibición técnica y arquitectónica (*Showcase*) con fines demostrativos y de portafolio. El código fuente completo, esquemas internos de base de datos y algoritmos de balance financiero se mantienen en repositorios privados bajo estrictos acuerdos de propiedad intelectual. **Todos los nombres, documentos de identidad, números telefónicos y cifras monetarias mostrados en las capturas son datos de prueba ficticios (Mock Data)** generados exclusivamente para entornos de homologación y testing, sin comprometer información real de clientes ni empresas.

---

## 💻 1. Ecosistema Web: Panel Administrativo (React + Tailwind)

El panel de administración para propietarios y auditores fue concebido bajo una arquitectura moderna **Mobile-First** con un sistema de diseño estructurado en **Bento Grid**. Esta disposición garantiza alta densidad de información financiera en tiempo real, adaptabilidad responsiva absoluta y dashboards analíticos claros sin saturación visual.

### Métricas y Reportes

<div align="center">
  <img src="assets/mockups/Reportes.png" width="49%" alt="Métricas" />
  <img src="assets/mockups/reportes2.png" width="49%" alt="Analítica Detallada" />
</div>

### Vista de Clientes y Auditoría
Módulo central del administrador para consultar el listado global de prestatarios asignados a cada ruta y auditar en profundidad el historial de crédito detallado, cuotas saldadas y estado de la deuda de cada cliente.

<div align="center">
  <img src="assets/mockups/clientes.png" width="49%" alt="Lista de Clientes" />
  <img src="assets/mockups/cliente_cartulina.png" width="49%" alt="Detalle de Cliente (Vista Admin)" />
</div>

### Monitoreo de Cobros (Cobros de Hoy)
Esta sección permite hacer seguimiento y control en tiempo real del dinero recaudado por los cobradores en sus rutas diarias, verificando abonos procesados, pendientes y cierres de jornada.

<div align="center">
  <img src="assets/mockups/cobros_de_hoy.png" width="80%" alt="Cobros de Hoy" />
</div>

### Recibo Digital Público (B2B2C)
Comprobante digital interactivo accesible mediante enlaces tokenizados seguros (*Magic Links*) enviados al cliente por WhatsApp. Permite a los usuarios consultar su saldo y cuotas en tiempo real con cifrado TLS y de solo lectura.

<div align="center">
  <img src="assets/mockups/recibo_digital.jpg" width="40%" alt="Recibo Público" />
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

  <video src="/assets/videos/general.mp4" width="35%" autoplay loop muted playsinline></video>
  <video src="/assets/videos/inico.mp4" width="35%" autoplay loop muted playsinline></video>

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

## 🛠️ 3. Arquitectura Asíncrona y Stack Tecnológico

El ecosistema CrediTrack está diseñado como una plataforma de alto rendimiento basada en arquitectura **100% Asíncrona (Event-Driven e I/O No Bloqueante)**, desacoplando tareas pesadas de los ciclos de renderizado tanto en la nube como en dispositivos cliente:

* **Backend No Bloqueante:** Construido sobre **Python (FastAPI)** aprovechando el loop nativo `async`/`await` de `uvloop` y el driver asíncrono de alto desempeño **Asyncpg** para PostgreSQL. Esto permite gestionar miles de conexiones y peticiones concurrentes de sincronización sin incurrir en bloqueos por I/O ni saturación de hilos del servidor.
* **Mobile Fluido (Dart Isolates):** La aplicación móvil delega el procesamiento intensivo de lectura/escritura en **Drift / SQLite** y la resolución de lotes de sincronización a subprocesos o hilos secundarios en segundo plano (**Dart Isolates**). Con esta separación de carga, el hilo principal de renderizado (*UI Thread*) se mantiene libre, garantizando una experiencia visual a **60 FPS sostenidos sin jank**.
* **🔥 Feature Destacada: Aprovisionamiento Asíncrono (Magic Links):**
  Para preservar la seguridad operativa y eliminar fricciones, los administradores no configuran contraseñas manuales ni credenciales compartidas para sus cobradores. El sistema despacha una solicitud asíncrona que genera un token criptográfico de un solo uso con firma temporal de expiración y construye un enlace seguro (*Magic Link*). Al acceder, el trabajador define sus credenciales y el backend orquesta el aprovisionamiento de la cuenta, roles y asignación de rutas en segundo plano a través de colas de tareas eficientes.

### Diagrama de Topología Asíncrona y Aprovisionamiento

```mermaid
flowchart TD
    %% ─────────────────────────────────────────────────────────────
    %% CAPA CLIENTE: WEB ADMIN & MOBILE
    %% ─────────────────────────────────────────────────────────────
    subgraph Frontend["Capa Cliente (Frontend & Mobile)"]
        direction TB

        subgraph WebAdmin["Panel Administrador Web"]
            AdminUI["Web Admin Dashboard<br/><b>React 19 + Tailwind CSS</b>"]
            ActionTrigger["Evento Asíncrono:<br/><b>Generar Link de Invitación</b>"]
            AdminUI --> ActionTrigger
        end

        subgraph MobileApp["App Móvil Cobrador"]
            MobileUI["UI Móvil a 60 FPS<br/><b>Flutter (UI Thread)</b>"]
            Isolates["Dart Isolates (Background)<br/><b>Lectura SQLite & Sync Sin Bloqueo</b>"]
            MagicActivation["Activación vía Magic Link<br/><b>Configuración de Credenciales</b>"]

            MobileUI <--> Isolates
            MagicActivation --> MobileUI
        end
    end

    %% ─────────────────────────────────────────────────────────────
    %% CAPA DE RED / APIS
    %% ─────────────────────────────────────────────────────────────
    ActionTrigger == "POST /api/workers/invite<br/>(HTTPS / Non-blocking)" ==> AsyncAPI
    MagicActivation == "POST /api/workers/activate<br/>(One-Time Crypto Token)" ==> AsyncAPI

    %% ─────────────────────────────────────────────────────────────
    %% CAPA BACKEND & COLAS DE TAREAS
    %% ─────────────────────────────────────────────────────────────
    subgraph Backend["Capa de Backend Asíncrono (FastAPI + Docker)"]
        direction TB

        AsyncAPI["Servidor API No Bloqueante<br/><b>FastAPI (Async Loop / uvloop)</b>"]
        WorkerManager["Background Worker / Gestor de Tokens<br/><b>Generación Criptográfica & Colas</b>"]
        AsyncPool["Pool Asíncrono de Conexiones<br/><b>Asyncpg Connection Pool</b>"]

        AsyncAPI <--> WorkerManager
        AsyncAPI <--> AsyncPool
    end

    %% ─────────────────────────────────────────────────────────────
    %% CAPA DE PERSISTENCIA
    %% ─────────────────────────────────────────────────────────────
    subgraph Storage["Capa de Persistencia"]
        direction TB
        CentralDB[("Base de Datos Central<br/><b>PostgreSQL (Cloud SaaS)</b>")]
        LocalDB[("Base de Datos Local Offline<br/><b>Drift / SQLite</b>")]
    end

    AsyncPool <== "I/O Concurrente en Milisegundos" ==> CentralDB
    Isolates <--> LocalDB

    %% ─────────────────────────────────────────────────────────────
    %% ESTILOS VISUALES (CLASSDEF)
    %% ─────────────────────────────────────────────────────────────
    classDef webStyle fill:#0F172A,stroke:#38BDF8,stroke-width:2px,color:#F8FAFC
    classDef mobileStyle fill:#064E3B,stroke:#10B981,stroke-width:2px,color:#ECFDF5
    classDef apiStyle fill:#1E1B4B,stroke:#818CF8,stroke-width:2px,color:#EEF2FF
    classDef workerStyle fill:#431407,stroke:#FB923C,stroke-width:2px,color:#FFF7ED
    classDef dbStyle fill:#18181B,stroke:#F59E0B,stroke-width:2px,color:#FEF3C7
    classDef containerGroup fill:#0B131B,stroke:#334155,stroke-width:1.5px,color:#94A3B8

    class AdminUI,ActionTrigger webStyle
    class MobileUI,Isolates,MagicActivation mobileStyle
    class AsyncAPI,AsyncPool apiStyle
    class WorkerManager workerStyle
    class CentralDB,LocalDB dbStyle
    class Frontend,Backend,Storage,WebAdmin,MobileApp containerGroup
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
