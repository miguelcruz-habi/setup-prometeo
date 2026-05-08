<!-- gsd-project-start source:PROJECT.md -->
## Project

**Portal de Prometeo**

Portal interno de Habi (real estate, Colombia/México) que documenta y habilita el framework **Prometeo**: un sistema para que personas operativas no técnicas construyan automatizaciones en Apps Script con asistencia de IA. Es un hub server-rendered con 7 secciones (Home, Accesos previos, Ruta de aprendizaje, Setup del computador, Pre work de negocio, Construye, Paso a producción) que viven en un único Web App de Apps Script. Es **dogfooding intencional**: el portal que enseña a usar el framework está construido con el propio framework.

**Core Value:** Una persona operativa de Habi llega al portal por primera vez y, **siguiendo las secciones en orden**, puede construir y desplegar a producción su primera automatización en Apps Script — sin necesitar a un desarrollador para configurar el setup, entender la ruta de aprendizaje, o pasar a producción.

### Constraints

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
<!-- gsd-project-end -->

<!-- gsd-stack-start source:STACK.md -->
## Technology Stack

Technology stack not yet documented. Will populate after codebase mapping or first phase.
<!-- gsd-stack-end -->

<!-- gsd-conventions-start source:CONVENTIONS.md -->
## Conventions

Conventions not yet established. Will populate as patterns emerge during development.
<!-- gsd-conventions-end -->

<!-- gsd-architecture-start source:ARCHITECTURE.md -->
## Architecture

Architecture not yet mapped. Follow existing patterns found in the codebase.
<!-- gsd-architecture-end -->

<!-- gsd-workflow-start source:GSD defaults -->
## GSD Workflow Enforcement

Before using Edit, Write, or other file-changing tools, start work through a GSD command so planning artifacts and execution context stay in sync.

Use these entry points:
- `/gsd-quick` for small fixes, doc updates, and ad-hoc tasks
- `/gsd-debug` for investigation and bug fixing
- `/gsd-execute-phase` for planned phase work

Do not make direct repo edits outside a GSD workflow unless the user explicitly asks to bypass it.
<!-- gsd-workflow-end -->



<!-- gsd-profile-start -->
## Developer Profile

> Profile not yet configured. Run `/gsd-profile-user` to generate your developer profile.
> This section is managed by `generate-claude-profile` -- do not edit manually.
<!-- gsd-profile-end -->
