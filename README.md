# CrediTrack SaaS - Plataforma Fintech de Recaudo en Terreno

<div align="center">
  <img src="assets/mockups/inicio.png" width="65%" alt="Dashboard Web" />
  <img src="assets/mockups/off-line.jpg" width="30%" alt="App Móvil Offline" />
</div>

<br />

**CrediTrack** es una plataforma SaaS B2B integral y de misión crítica diseñada específicamente para microfinancieras, empresas de crédito independiente y equipos de cobranza de campo. El sistema digitaliza y automatiza el ciclo operativo completo: administración de cartera, liquidación de cuotas, trazabilidad de cobradores en ruta y sincronización de pagos bajo condiciones de red nula o intermitente (*Offline-First*).

> **Nota sobre el código fuente:** Este repositorio constituye una exhibición técnica y arquitectónica (*Showcase*) con fines demostrativos y de portafolio. El código fuente completo, esquemas internos de base de datos y algoritmos de balance financiero se mantienen en repositorios privados bajo estrictos acuerdos de propiedad intelectual y confidencialidad comercial.

---

## 2. Panel Administrativo Web (React + Tailwind CSS)

El panel de administración para propietarios y auditores fue concebido bajo una arquitectura moderna **Mobile-First** con un sistema de diseño estructurado en **Bento Grid**. Esta disposición garantiza alta densidad de información financiera en tiempo real, adaptabilidad responsiva absoluta y dashboards analíticos claros sin saturación visual.

### Métricas y Reportes

<div align="center">
  <img src="assets/mockups/Reportes.png" width="49%" alt="Métricas" />
  <img src="assets/mockups/reportes2.png" width="49%" alt="Analítica Detallada" />
</div>

### Gestión de Cartera y Clientes

<div align="center">
  <img src="assets/mockups/clientes.png" width="49%" alt="Lista de Clientes" />
  <img src="assets/mockups/cobros_de _hoy.png" width="49%" alt="Cobros de Hoy" />
</div>

### Cartulina Pública (B2B2C)
Comprobante digital interactivo accesible mediante enlaces tokenizados seguros (*Magic Links*). Los clientes finales pueden auditar la transparencia de su crédito en tiempo real, revisar sus cuotas saldadas, días de gracia y compartir su cartulina electrónica directamente por WhatsApp.

<div align="center">
  <img src="assets/mockups/cliente_cartulina.png" width="80%" alt="Cartulina Pública Cliente" />
</div>

### Configuración de Negocio
Centro de control administrativo con soporte multi-rol (Dueño / Cobrador). Permite parametrizar tasas de interés por defecto, modalidades de liquidación diaria/semanal, días festivos y umbrales de alerta de caja.

<div align="center">
  <img src="assets/mockups/configuracion.png" width="80%" alt="Configuración de Negocio" />
</div>

---

## 3. Aplicación Móvil para Cobradores (Flutter + Drift + Riverpod)

Herramienta nativa para operadores en terreno diseñada con una arquitectura de resiliencia extrema **Offline-First**. Garantiza que el cobrador registre transacciones, consulte saldos y realice cálculos financieros instantáneos incluso en zonas rurales sin cobertura móvil.

### 🎥 Experiencia de Usuario (Motion UI & Glassmorphism)
La aplicación implementa microinteracciones fluidas a 60 FPS, navegación háptica con barra flotante desacoplada y estética *Glassmorphism* con desenfoque de fondo en tiempo real para optimizar la ergonomía y velocidad de captura en calle.

<div align="center">
  <video src="assets/videos/demo_navegacion.mp4" width="35%" controls autoplay loop muted></video>
  <video src="assets/videos/demo_cobro.mp4" width="35%" controls autoplay loop muted></video>
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

## 4. Stack Tecnológico y Arquitectura

* **Frontend Web:** React 19, Vite, Tailwind CSS, Lucide Icons, Framer Motion.
* **Aplicación Móvil:** Flutter (Clean Architecture), Riverpod (State Management), Drift / SQLite (Base de datos local), Flutter Secure Storage.
* **Backend & Servicios:** Python (FastAPI / SQLAlchemy), Pydantic, Uvicorn, Docker & Docker Compose.
* **Bases de Datos & Almacenamiento:** PostgreSQL en la nube (instancia SaaS centralizada) y SQLite embebido en dispositivos cliente.
* **Seguridad & Protocolos:** JWT con expiración estricta, Hash bcrypt, Cifrado TLS/HTTPS, esquemas Zero-Trust para enlaces públicos.

<div align="center">

![Diagrama de Arquitectura](assets/architecture/diagrama_arquitectura.md)

</div>

---

## 5. Retos de Ingeniería Resueltos

1. **Resolución de Conflictos Offline y Colas Idempotentes:**
   Implementación de un motor de sincronización bidireccional con marcas de tiempo lógicas y hashes de transacción. Cuando múltiples abonos se registran sin conexión en zonas remotas, el backend procesa las colas asegurando idempotencia estricta, conciliando balances sin duplicaciones ni inconsistencias contables.

2. **Prevención de Layout Blowouts en Motores Móviles Restrictivos:**
   Optimización del motor de renderizado CSS/Tailwind para mitigar fallos de desbordamiento horizontal (*viewport blowout*) en navegadores in-app y navegadores propietarios (ej. WebView de Huawei). Se implementaron contenedores estrictos con límites `overflow-x-hidden`, empaquetado de texto forzado (`break-words`) y dimensionamiento elástico no destructivo en componentes gráficos.

3. **Seguridad de Sesiones en Cold Start:**
   Garantía de inicialización atómica de credenciales en arranques en frío de la aplicación móvil. El pipeline de inicio verifica la integridad del almacenamiento seguro local antes de montar el árbol de widgets de Riverpod, evitando parpadeos de redirección no autenticada y asegurando persistencia transparente en modo avión.

---

## 📄 Licencia

Este es un proyecto comercial de código cerrado. Todos los derechos reservados © 2026 Carlos Acosta. Queda prohibida la reproducción total o parcial del diseño, material multimedia y la arquitectura aquí expuesta sin autorización expresa.

