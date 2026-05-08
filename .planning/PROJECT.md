# Portal de Prometeo

## What This Is

Portal interno de Habi (real estate, Colombia/México) que documenta y habilita el framework **Prometeo**: un sistema para que personas operativas no técnicas construyan automatizaciones en Apps Script con asistencia de IA. Es un hub server-rendered con 7 secciones (Home, Accesos previos, Ruta de aprendizaje, Setup del computador, Pre work de negocio, Construye, Paso a producción) que viven en un único Web App de Apps Script. Es **dogfooding intencional**: el portal que enseña a usar el framework está construido con el propio framework.

## Core Value

Una persona operativa de Habi llega al portal por primera vez y, **siguiendo las secciones en orden**, puede construir y desplegar a producción su primera automatización en Apps Script — sin necesitar a un desarrollador para configurar el setup, entender la ruta de aprendizaje, o pasar a producción.

## Requirements

### Validated

<!-- Shipped and confirmed valuable. -->

(None yet — ship to validate)

### Active

<!-- Current scope. Building toward these. Built section by section, in order. -->

- [ ] **Web App base**: Una Web App de Apps Script con sidebar fijo de navegación, server-rendered, que valida `@habi.co` vía `Session.getActiveUser()` y bloquea cualquier otro dominio.
- [ ] **Ruteo `?seccion=`**: Deep-link funcional a las 7 secciones con renderizado server-side por sección.
- [ ] **Sección Home**: Frase de qué es Prometeo, lista de casos en producción (Gestión Portafolio, Auditoría Ocaltzin, Cupos, Ticketera), próxima Office Hour, link a Slack `#prometeo-ayuda`, CTA "Pedir accesos".
- [ ] **Sección Accesos previos**: Cursor + GitHub + link a ticketera de DevOps.
- [ ] **Sección Ruta de aprendizaje**: Cursos de Platzi en dos bloques: "antes de tocar máquina" y "en paralelo al desarrollo".
- [ ] **Sección Setup del computador**: Paso a paso para clasp, Git, ambientes dev/prod (alineado con el skill global de Cursor).
- [ ] **Sección Pre work de negocio**: Plantilla de PRD + 3 criterios de autoevaluación (problema con horas medibles, alcance ≤1 mes, usado por >1 persona).
- [ ] **Sección Construye**: Ciclo Get Shit Done paso a paso.
- [ ] **Sección Paso a producción**: Ambientes dev/prod, comandos clasp/git, checklist de lineamientos (manejo de datos sensibles, BigQuery, API keys, qué NO hacer).
- [ ] **CTA "Pedir accesos"**: Formulario server-side que valida el usuario, dispara notificación a Slack `#prometeo-ayuda` vía webhook, y persiste log mínimo en `Logger`.
- [ ] **Deploy dev/prod separados**: Aprovechar el script existente `scripts/push.js` con `npm run push:dev` y `npm run push:prod` apuntando a Script IDs distintos.
- [ ] **Configuración por `PropertiesService`**: `DOMINIO_PERMITIDO`, `SLACK_WEBHOOK_URL`, `CANAL_NOTIFICACION` configurables por ambiente sin tocar código.
- [ ] **Estilo visual sobrio**: Tipografía system-ui o Inter, paleta neutra, sin gradientes, sin glassmorphism, sin emojis decorativos.

### Out of Scope

<!-- Explicit boundaries. Includes reasoning to prevent re-adding. -->

- **Google Sheets como CMS** — el equipo decidió que el contenido vive en código (constantes JS por sección). Trade-off aceptado: cualquier cambio de contenido implica `git commit` + `npm run push:prod`. Se prioriza simplicidad y latencia sobre edición sin código.
- **SPA, React o cualquier framework JS pesado en el cliente** — restricción técnica no negociable. Todo es server-rendered con `HtmlService`.
- **Roles dentro del portal** (admin/lector) — todos los usuarios `@habi.co` autenticados son lectores. No hay diferencia de permisos.
- **Múltiples dominios** (`@habi.com.mx`, etc.) — V1 solo `@habi.co`. Si se requiere México, se agrega después editando `DOMINIO_PERMITIDO` en `PropertiesService` (la variable ya está parametrizada).
- **Persistencia de solicitudes "Pedir accesos" en Sheet/DB** — V1 solo notifica a Slack. Si Slack se pierde, se pierde la solicitud. Se acepta porque el volumen esperado es bajo y los humanos del canal hacen seguimiento.
- **Office Hour leída en vivo desde Google Calendar** — V1 hard-coded en `ContenidoHome.js`. Se reevaluará si el equipo cambia OH con frecuencia suficiente para justificar el scope extra.
- **Librerías externas** (npm packages, CDNs en HTML) — restricción técnica. Solo se permiten salvo justificación explícita.

## Context

- **Audiencia**: personas operativas de Habi en Colombia y México que **no saben programar**. El tono y la ergonomía deben tratarlas como inteligentes pero no técnicas.
- **Contexto cultural**: Habi es real estate. Hay casos en producción reales (Gestión Portafolio, Auditoría Ocaltzin, Cupos, Ticketera) que son la prueba social del Portal.
- **El nombre Prometeo** viene del mito griego (robar el fuego de los dioses para entregárselo a los mortales). El framework habilita que la gente operativa "construya con sus propias manos" en lugar de depender de devs. El Portal debe respirar esa mentalidad.
- **Repo base**: el directorio actual `setup-prometeo` ya tiene un starter de Apps Script + clasp + scripts de deploy multi-ambiente. Sobre eso construimos el Portal.
- **Existe un repo hermano** llamado `appscript-prometeo` que sirve de **referencia de estructura** (módulos por responsabilidad: config, lógica principal, integraciones, sheets, etc.). El Portal sigue ese patrón.
- **Slack `#prometeo-ayuda`** es el canal de soporte. La integración con Slack es vía webhook (URL guardada en `PropertiesService`).
- **Cursor + GitHub** son las herramientas que cualquier construye-Prometeo necesita; gestionarlos es un paso previo (sección "Accesos previos").
- **Office Hours** son sesiones recurrentes; aparecen en Home como "próxima OH".
- **Deploy multi-ambiente** ya está resuelto en el starter (`scripts/push.js` + `environments.json`).

## Constraints

- **Tech stack**: Apps Script (HtmlService server-rendered, sin frameworks JS). No SPA, no React, no Vue, no Svelte. Razón: dogfooding del propio framework que el Portal enseña.
- **Tech stack**: Backend = Apps Script puro. CMS = constantes JS en el repo (no Sheet, no DB externa). Razón: simplicidad, sin latencia de Sheet, sin scopes de Sheets.
- **Tech stack**: Sin librerías externas (npm o CDN) salvo justificación explícita. Razón: minimizar superficie, mantener portabilidad clasp/GAS.
- **Auth**: Google nativo vía `Session.getActiveUser()`, dominio único parametrizable (`DOMINIO_PERMITIDO` en `PropertiesService`, default `habi.co`). Razón: no reinventar identidad, aprovechar SSO de Google Workspace de Habi.
- **Versionado**: clasp + Git, ambientes `dev` y `prod` separados con scripts ya existentes (`npm run push:dev`, `npm run push:prod`). Razón: paridad con cómo trabaja el resto del framework Prometeo.
- **Idioma**: comentarios, variables, contenido y nombres de archivos en español. Razón: el equipo trabaja en español; reduce la fricción cognitiva.
- **Estilo visual**: sobrio, bien tipografiado. Sin gradientes morados, sin glassmorphism, sin emojis decorativos, sin "look de IA". Razón: el Portal habla a operadores de negocio; debe verse como herramienta interna seria.
- **Estructura de código**: archivos separados por responsabilidad (referencia: repo `appscript-prometeo`). Carpetas locales: `contenido/`, `secciones/`, `partials/`, `estilos/`, `integraciones/`. Razón: legibilidad y onboarding.
- **Forma de trabajo**: una sección a la vez, en el orden listado, partiendo por Home. Sin escribir código sin estructura aprobada previamente. Sin asumir contenido — preguntar antes de inventar.
- **Forma de trabajo**: no optimizar prematuramente. Lo simple primero; refactor cuando duela.
- **Documentación de decisiones**: comentarios cortos solo donde el "por qué" no es obvio. No narrar el "qué".

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Construir el Portal en este mismo repo (`setup-prometeo`) | Aprovecha el starter ya configurado (clasp, deploy dev/prod, .gitignore). Evita duplicar setup. | — Pending |
| CMS en código (constantes JS) en vez de Google Sheet | Trade-off: pierde edición sin código, gana simplicidad y latencia. El equipo prefiere `git push` controlado sobre velocidad de edición de no-devs. | — Pending |
| Una sola Web App con sidebar fijo y `?seccion=` | Mejor UX (sidebar siempre visible, deep-linkable) que múltiples Web Apps separadas. Mantiene server-rendering puro. | — Pending |
| Auth por dominio único parametrizable (`DOMINIO_PERMITIDO`) en vez de hard-coded `@habi.co` | Permite agregar `@habi.com.mx` después sin tocar código (solo `PropertiesService`). | — Pending |
| "Pedir accesos" notifica a Slack vía webhook, sin persistir en Sheet/DB | V1 minimalista. El humano de `#prometeo-ayuda` da seguimiento. Si crece el volumen se reevalúa. | — Pending |
| Construir sección por sección en el orden Home → Accesos → Ruta → Setup → Pre work → Construye → Paso a producción | Cada sección entrega valor independiente; el orden refleja el journey del usuario operativo. | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-05-07 after initialization*
