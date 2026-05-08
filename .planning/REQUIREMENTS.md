# Requirements: Portal de Prometeo

**Defined:** 2026-05-07
**Core Value:** Una persona operativa de Habi llega al portal por primera vez y, siguiendo las secciones en orden, puede construir y desplegar a producción su primera automatización en Apps Script — sin necesitar a un desarrollador.

## v1 Requirements

Requisitos de la primera versión. Todos mapean a fases del roadmap.

### Infraestructura (INFRA)

- [ ] **INFRA-01**: La Web App responde a `doGet(e)` desde una URL pública de Apps Script.
- [ ] **INFRA-02**: La Web App valida que `Session.getActiveUser().getEmail()` termine en el dominio configurado en `PropertiesService.getScriptProperties().getProperty('DOMINIO_PERMITIDO')` (default `habi.co`); cualquier otro dominio recibe una pantalla de "no autorizado" en español.
- [ ] **INFRA-03**: El parámetro de query `?seccion=<nombre>` enruta al renderer correcto; ausencia o valor inválido cae a `home`.
- [ ] **INFRA-04**: El layout server-rendered incluye un sidebar fijo con links a las 7 secciones; la sección activa se marca visualmente.
- [ ] **INFRA-05**: Las claves de configuración (`DOMINIO_PERMITIDO`, `SLACK_WEBHOOK_URL`, `CANAL_NOTIFICACION`) se leen exclusivamente vía `PropertiesService` (nunca hard-coded).
- [ ] **INFRA-06**: El proyecto se despliega a `dev` con `npm run push:dev` y a `prod` con `npm run push:prod` apuntando a Script IDs separados.
- [ ] **INFRA-07**: El `appsscript.json` declara únicamente los scopes `userinfo.email` y `script.external_request` (mínimos).

### Estilo visual (ESTILO)

- [ ] **ESTILO-01**: La hoja de estilos define una paleta neutra (negros, grises, un acento sobrio) sin gradientes, glassmorphism ni efectos visuales decorativos.
- [ ] **ESTILO-02**: La tipografía es system-ui (o Inter como fallback), con jerarquía clara (h1 / h2 / cuerpo / metadata).
- [ ] **ESTILO-03**: Los estilos viven en un único archivo `estilos/Estilos.html` incluido vía `HtmlService.createTemplateFromFile().include('estilos/Estilos')`.
- [ ] **ESTILO-04**: Las 7 secciones usan los mismos componentes de UI base (sin estilos por sección que rompan la consistencia).

### Sección Home (HOME)

- [ ] **HOME-01**: La Home muestra una frase de qué es Prometeo (un párrafo, en español).
- [ ] **HOME-02**: La Home lista los casos en producción actuales (Gestión Portafolio, Auditoría Ocaltzin, Cupos, Ticketera) con nombre y una línea de descripción cada uno.
- [ ] **HOME-03**: La Home muestra la próxima Office Hour (fecha, hora, link de la sesión) leída de una constante en `ContenidoHome.js`.
- [ ] **HOME-04**: La Home muestra un link al canal de Slack `#prometeo-ayuda`.
- [ ] **HOME-05**: La Home muestra el CTA "Pedir accesos" como botón prominente que abre el formulario.

### Sección Accesos previos (ACCESOS)

- [ ] **ACCESOS-01**: La sección documenta cómo solicitar acceso a Cursor (paso a paso, link a portal interno o ticketera).
- [ ] **ACCESOS-02**: La sección documenta cómo solicitar acceso a GitHub (paso a paso, link a la org de Habi).
- [ ] **ACCESOS-03**: La sección incluye un link directo a la ticketera de DevOps para abrir solicitudes.

### Sección Ruta de aprendizaje (RUTA)

- [ ] **RUTA-01**: La sección presenta dos bloques claramente separados: "Antes de tocar máquina" y "En paralelo al desarrollo".
- [ ] **RUTA-02**: Cada bloque lista cursos de Platzi con nombre, link y duración estimada, leídos de constantes en `ContenidoRuta.js`.

### Sección Setup del computador (SETUP)

- [ ] **SETUP-01**: La sección presenta un paso a paso ordenado para instalar y configurar `clasp`.
- [ ] **SETUP-02**: La sección presenta un paso a paso para configurar Git localmente (clone, identidad, ssh).
- [ ] **SETUP-03**: La sección documenta cómo crear y configurar ambientes `dev` y `prod` separados (alineado con el skill global de Cursor para Prometeo).
- [ ] **SETUP-04**: Los comandos shell se muestran en bloques de código copiables.

### Sección Pre work de negocio (PREWORK)

- [ ] **PREWORK-01**: La sección incluye una plantilla de PRD descargable o copiable (link a Google Doc plantilla, o texto inline en bloque copiable).
- [ ] **PREWORK-02**: La sección lista los 3 criterios de autoevaluación: (a) problema con horas medibles, (b) alcance ≤1 mes, (c) usado por >1 persona.
- [ ] **PREWORK-03**: Cada criterio se presenta con una pregunta concreta de auto-validación.

### Sección Construye (CONSTRUYE)

- [ ] **CONSTRUYE-01**: La sección presenta el ciclo Get Shit Done paso a paso, en orden secuencial.
- [ ] **CONSTRUYE-02**: Cada paso del ciclo describe qué hacer y cómo se relaciona con el siguiente.

### Sección Paso a producción (PROD)

- [ ] **PROD-01**: La sección documenta la diferencia entre ambientes `dev` y `prod` (cuándo usar cada uno).
- [ ] **PROD-02**: La sección lista los comandos `clasp` y `git` para llevar código a producción, en orden.
- [ ] **PROD-03**: La sección incluye un checklist de lineamientos de manejo de datos sensibles (qué se puede y qué NO).
- [ ] **PROD-04**: La sección documenta el manejo de API keys (`PropertiesService`, no commitear, etc.).
- [ ] **PROD-05**: La sección incluye una sección "Qué NO hacer" con anti-patrones explícitos (BigQuery sin permisos, datos personales en logs, etc.).

### CTA "Pedir accesos" (CTA)

- [ ] **CTA-01**: El formulario "Pedir accesos" se abre desde Home y desde la sección Accesos.
- [ ] **CTA-02**: El formulario captura nombre, equipo y motivo de la solicitud (campos mínimos).
- [ ] **CTA-03**: El submit valida sesión activa de `@habi.co` server-side antes de procesar.
- [ ] **CTA-04**: Al validar, dispara un POST a `SLACK_WEBHOOK_URL` con un payload que incluye email del solicitante, nombre, equipo, motivo y timestamp.
- [ ] **CTA-05**: El usuario recibe confirmación visual (mensaje server-rendered) de que la solicitud fue enviada.
- [ ] **CTA-06**: Si el webhook falla (no-2xx, timeout), el usuario ve un mensaje de error claro y la solicitud queda registrada en `Logger.log` con suficiente contexto para reintento manual.

## v2 Requirements

Diferidos a versiones futuras. Acknowledgeados pero no en el roadmap actual.

### Multi-dominio y México

- **MX-01**: Soporte para `@habi.com.mx` (agregando dominio a `DOMINIO_PERMITIDO` o cambiando esquema a lista de dominios permitidos).
- **MX-02**: Variantes de contenido por país (Office Hours regional, casos en producción regional).

### Office Hours dinámicas

- **OH-01**: Próxima Office Hour leída en vivo desde Google Calendar (en lugar de constante en código).

### Persistencia de solicitudes

- **CTA-V2-01**: Persistir solicitudes "Pedir accesos" en una tab de Google Sheet además de notificar a Slack (para auditoría y seguimiento).

### Métricas

- **MET-01**: Tracking básico de visitas por sección (qué secciones usan más, cuáles abandonan).

## Out of Scope

Excluidos explícitamente. Documentados para evitar scope creep.

| Feature | Reason |
|---------|--------|
| Google Sheets como CMS | Decisión consciente: el contenido vive en código. Trade-off de simplicidad vs edición sin código. |
| SPA / React / Vue / cualquier framework JS pesado en cliente | Restricción técnica no negociable; dogfooding del propio framework Prometeo. |
| Roles dentro del Portal (admin/lector) | Todos los usuarios `@habi.co` son lectores por igual. |
| Múltiples Web Apps (una por sección) | Decisión arquitectónica: una sola Web App con sidebar y `?seccion=`. |
| Login con email/password o OAuth no-Google | Habi usa Google Workspace; reusar SSO. |
| Librerías npm o CDNs externos en HTML | Restricción de simplicidad y portabilidad clasp/GAS. |
| Edición de contenido desde el Portal mismo | El contenido se edita en código y se despliega con `clasp`. |
| Diseño responsive avanzado para móvil | El Portal se consume principalmente en desktop. Móvil debe ser legible pero no optimizado. |
| Internacionalización (i18n) | Todo en español. |
| Emojis decorativos, gradientes, glassmorphism | Restricción de estilo. |

## Traceability

Mapeo de requisitos a fases. Cada requisito v1 mapea a exactamente una fase.

| Requirement | Phase | Status |
|-------------|-------|--------|
| INFRA-01 | Phase 1 | Pending |
| INFRA-02 | Phase 1 | Pending |
| INFRA-03 | Phase 1 | Pending |
| INFRA-04 | Phase 1 | Pending |
| INFRA-05 | Phase 1 | Pending |
| INFRA-06 | Phase 1 | Pending |
| INFRA-07 | Phase 1 | Pending |
| ESTILO-01 | Phase 1 | Pending |
| ESTILO-02 | Phase 1 | Pending |
| ESTILO-03 | Phase 1 | Pending |
| ESTILO-04 | Phase 1 | Pending |
| HOME-01 | Phase 2 | Pending |
| HOME-02 | Phase 2 | Pending |
| HOME-03 | Phase 2 | Pending |
| HOME-04 | Phase 2 | Pending |
| HOME-05 | Phase 2 | Pending |
| CTA-01 | Phase 2 | Pending |
| CTA-02 | Phase 2 | Pending |
| CTA-03 | Phase 2 | Pending |
| CTA-04 | Phase 2 | Pending |
| CTA-05 | Phase 2 | Pending |
| CTA-06 | Phase 2 | Pending |
| ACCESOS-01 | Phase 3 | Pending |
| ACCESOS-02 | Phase 3 | Pending |
| ACCESOS-03 | Phase 3 | Pending |
| RUTA-01 | Phase 4 | Pending |
| RUTA-02 | Phase 4 | Pending |
| SETUP-01 | Phase 5 | Pending |
| SETUP-02 | Phase 5 | Pending |
| SETUP-03 | Phase 5 | Pending |
| SETUP-04 | Phase 5 | Pending |
| PREWORK-01 | Phase 6 | Pending |
| PREWORK-02 | Phase 6 | Pending |
| PREWORK-03 | Phase 6 | Pending |
| CONSTRUYE-01 | Phase 7 | Pending |
| CONSTRUYE-02 | Phase 7 | Pending |
| PROD-01 | Phase 8 | Pending |
| PROD-02 | Phase 8 | Pending |
| PROD-03 | Phase 8 | Pending |
| PROD-04 | Phase 8 | Pending |
| PROD-05 | Phase 8 | Pending |

**Coverage:**
- v1 requirements: 41 total
- Mapped to phases: 41 ✓
- Unmapped: 0

**Distribución por fase:**

| Phase | Name | Reqs | Count |
|-------|------|------|-------|
| 1 | Cimientos (Infraestructura y Estilo) | INFRA-01..07, ESTILO-01..04 | 11 |
| 2 | Home + CTA "Pedir accesos" | HOME-01..05, CTA-01..06 | 11 |
| 3 | Accesos previos | ACCESOS-01..03 | 3 |
| 4 | Ruta de aprendizaje | RUTA-01..02 | 2 |
| 5 | Setup del computador | SETUP-01..04 | 4 |
| 6 | Pre work de negocio | PREWORK-01..03 | 3 |
| 7 | Construye | CONSTRUYE-01..02 | 2 |
| 8 | Paso a producción | PROD-01..05 | 5 |

---
*Requirements defined: 2026-05-07*
*Last updated: 2026-05-07 after roadmap creation (41/41 mapped to 8 phases)*
