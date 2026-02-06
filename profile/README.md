<!-- README.md — Organización GitHub: gloriafinance -->

<p align="center">
  <img src="https://gloriafinance.com.br/assets/logoHorizontal-C8Lnr0Bn.png" alt="Glória Finance" width="420" />
</p>

<h1 align="center">Glória Finance</h1>

<p align="center">
  Plataforma de gestión financiera y administrativa para iglesias — enfocada en <strong>orden</strong>, <strong>transparencia</strong> y <strong>buena administración</strong>.
</p>

<p align="center">
  <a href="https://gloriafinance.com.br">Sitio web</a> •
  <a href="https://github.com/gloriafinance">GitHub</a>
</p>

---

## 🎯 Visión y propósito

**Glória Finance** existe para servir a las iglesias con una solución simple y robusta para:

- **Control financiero** (ingresos/egresos, cuentas, categorías, centros de costo)
- **Contribuciones** (diezmos, ofrendas, votos y otros tipos de contribución)
- **Organización administrativa** (registros, estructuras, permisos)
- **Patrimonio y gastos** (activos, compras, registros y trazabilidad)
- **Reportes** (DRE/resultado, flujo de caja, análisis por categoría y por período)
- **Operación ministerial** (base para módulos como agenda/eventos y rutinas)

Nuestro objetivo es entregar una experiencia consistente, auditable y fácil de usar — adaptada a la realidad de las iglesias.

---

## 🧩 Repositorios principales

### 1) API (Backend)
**Repositorio:** https://github.com/gloriafinance/gloria_finance_api

**Responsabilidad**
- Exponer los endpoints de la plataforma (autenticación, reglas de negocio y persistencia)
- Centralizar **dominios**, **casos de uso** y **políticas de seguridad**
- Garantizar consistencia en cálculos y operaciones (ej.: sumas y reglas por moneda)
- Integrar servicios externos (cuando aplique) y proveer eventos/colas (cuando aplique)

**Qué encontrarás aquí**
- Rutas/endpoints de la API
- Modelos/entidades y reglas de dominio
- Servicios de aplicación e integraciones
- Mecanismos de autenticación/autorización (RBAC/roles)
- Persistencia (ej.: MongoDB/Redis, según el proyecto)

---

### 2) Front (Aplicación cliente)
**Repositorio:** https://github.com/gloriafinance/gloria_finance_front

**Responsabilidad**
- Implementar la UI/UX de la plataforma (web/app) consumiendo la API
- Asegurar consistencia visual y de experiencia (multi-idioma, accesibilidad, estados de carga/error)
- Orquestar jornadas del usuario: dashboard, reportes, contribuciones, configuración, etc.
- Aplicar patrones de diseño, componentes y validaciones en el cliente

**Qué encontrarás aquí**
- Pantallas/flows del producto
- Componentización y design system (si aplica)
- Integración con API (HTTP client, interceptors, cache/estado)
- Internacionalización (pt-BR, es, en — cuando esté habilitado)
- Build/deploy del front

---

## 🏗️ Arquitectura (vista general)

Glória Finance está organizada como una arquitectura **Frontend ↔ API**, donde:

- El **frontend** (Flutter) implementa la experiencia de usuario y consume la API.
- La **API** concentra autenticación, reglas de negocio, persistencia y orquestación de procesos (incluyendo jobs asíncronos).
- La infraestructura se apoya en **MongoDB** (vía `MONGO_URI`), **Redis + BullMQ** para colas/jobs y **GCP Storage** para manejo de archivos (según configuración del backend).

### Componentes (según repositorios)

#### 1) Frontend — `gloria_finance_front`
- **Tecnología:** Flutter (Dart)
- **Targets:** estructura multiplataforma (carpetas `android/`, `ios/`, `web/`) → app/web desde el mismo código base.

Repo: https://github.com/gloriafinance/gloria_finance_front

#### 2) Backend/API — `gloria_finance_api`
- **Runtime:** Bun
- **Lenguaje:** TypeScript
- **Infra local:** Docker + Docker Compose
- **Base de datos:** MongoDB (configurado por `MONGO_URI`)
- **Cache/colas (jobs):** Redis + BullMQ
- **Archivos:** Google Cloud Storage (GCP Storage)

Repo: https://github.com/gloriafinance/gloria_finance_api

### Diagrama (alto nivel)

```text
┌─────────────────────────────────────────┐
│          Frontend (Flutter/Dart)        │
│        gloria_finance_front             │
│  UI/UX + navegación + i18n + estado     │
└───────────────────────┬─────────────────┘
                        │ HTTPS / JSON
                        ▼
┌─────────────────────────────────────────┐
│             API (Bun + TS)              │
│           gloria_finance_api            │
│ Auth + Casos de uso + Dominio + Jobs    │
└───────────────┬───────────────┬─────────┘
                │               │
                │               ├── GCP Storage (archivos)
                │
                ├── MongoDB (MONGO_URI)
                └── Redis + BullMQ (colas/jobs)

