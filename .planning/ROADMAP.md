# Roadmap: Portal de Prometeo

## Overview

El Portal de Prometeo se construye **una sección a la vez, en el orden del journey del usuario operativo**: primero los cimientos (Web App, sidebar, auth, deploy y estilo visual), luego cada sección de contenido en el orden en que la persona la consume — Home (con el CTA "Pedir accesos"), Accesos previos, Ruta de aprendizaje, Setup del computador, Pre work de negocio, Construye y Paso a producción. Cada fase entrega valor independiente y verificable: cuando la fase 2 cierra, la Home ya funciona en `dev` con CTA real a Slack; cuando la fase 8 cierra, una persona operativa puede recorrer el portal de principio a fin y desplegar su primera automatización a producción sin depender de un desarrollador.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [ ] **Phase 1: Cimientos (Infraestructura y Estilo)** - Web App con auth `@habi.co`, ruteo `?seccion=`, sidebar fijo, deploy dev/prod y paleta visual sobria.
- [ ] **Phase 2: Home + CTA "Pedir accesos"** - Primera sección visible: qué es Prometeo, casos en producción, próxima Office Hour, link a Slack y formulario funcional a `#prometeo-ayuda`.
- [ ] **Phase 3: Accesos previos** - Cómo solicitar Cursor, GitHub y abrir tickets en DevOps; reusa el CTA "Pedir accesos".
- [ ] **Phase 4: Ruta de aprendizaje** - Cursos de Platzi en dos bloques: antes de tocar máquina y en paralelo al desarrollo.
- [ ] **Phase 5: Setup del computador** - Paso a paso para clasp, Git y ambientes dev/prod, alineado con el skill global de Cursor.
- [ ] **Phase 6: Pre work de negocio** - Plantilla de PRD y 3 criterios de autoevaluación de scope.
- [ ] **Phase 7: Construye** - Ciclo Get Shit Done paso a paso, en orden secuencial.
- [ ] **Phase 8: Paso a producción** - Diferencia dev/prod, comandos clasp/git, checklist de datos sensibles, manejo de API keys y anti-patrones.

## Phase Details

### Phase 1: Cimientos (Infraestructura y Estilo)
**Goal**: Las personas con cuenta `@habi.co` acceden al Web App, navegan entre las 7 secciones desde un sidebar consistente, y el equipo despliega a `dev` y `prod` por separado con un estilo visual sobrio compartido.
**Depends on**: Nothing (first phase)
**Requirements**: INFRA-01, INFRA-02, INFRA-03, INFRA-04, INFRA-05, INFRA-06, INFRA-07, ESTILO-01, ESTILO-02, ESTILO-03, ESTILO-04
**Success Criteria** (what must be TRUE):
  1. Una persona con cuenta `@habi.co` abre la URL del Web App, ve la Home renderizada server-side y el sidebar fijo con links a las 7 secciones (la activa marcada visualmente).
  2. Una persona con dominio distinto a `@habi.co` ve una pantalla "no autorizado" en español y no accede al contenido del portal.
  3. Cambiar `?seccion=<nombre>` en la URL renderiza la sección correspondiente; ausencia o valor inválido cae a `home`.
  4. El equipo despliega a `dev` con `npm run push:dev` y a `prod` con `npm run push:prod`, apuntando a Script IDs distintos, con `DOMINIO_PERMITIDO`, `SLACK_WEBHOOK_URL` y `CANAL_NOTIFICACION` leídos vía `PropertiesService` y `appsscript.json` declarando solo los scopes mínimos (`userinfo.email`, `script.external_request`).
  5. Todas las pantallas comparten paleta neutra, tipografía system-ui/Inter con jerarquía clara y los mismos componentes UI base — sin gradientes, glassmorphism ni emojis decorativos, con estilos centralizados en `estilos/Estilos.html`.
**Plans**: TBD
**UI hint**: yes

### Phase 2: Home + CTA "Pedir accesos"
**Goal**: La Home presenta qué es Prometeo, casos en producción, próxima Office Hour y link a Slack, e incluye un CTA "Pedir accesos" que envía solicitudes reales a `#prometeo-ayuda` con manejo de errores.
**Depends on**: Phase 1
**Requirements**: HOME-01, HOME-02, HOME-03, HOME-04, HOME-05, CTA-01, CTA-02, CTA-03, CTA-04, CTA-05, CTA-06
**Success Criteria** (what must be TRUE):
  1. Una persona ve en la Home: una frase de qué es Prometeo, los 4 casos en producción (Gestión Portafolio, Auditoría Ocaltzin, Cupos, Ticketera) con descripción de una línea, y la próxima Office Hour con fecha, hora y link.
  2. La persona accede al canal Slack `#prometeo-ayuda` desde un link visible en la Home.
  3. La persona abre el formulario "Pedir accesos" desde el CTA prominente de la Home y captura nombre, equipo y motivo.
  4. Al enviar la solicitud (con sesión `@habi.co` validada server-side), llega un mensaje a Slack `#prometeo-ayuda` con email, nombre, equipo, motivo y timestamp; la persona ve un mensaje de confirmación server-rendered.
  5. Si el webhook falla (no-2xx o timeout), la persona ve un mensaje de error claro en pantalla y la solicitud queda registrada en `Logger.log` con suficiente contexto para reintento manual.
**Plans**: TBD
**UI hint**: yes

### Phase 3: Accesos previos
**Goal**: Una persona operativa entiende qué accesos necesita (Cursor, GitHub, ticketera de DevOps) y puede solicitarlos sin abandonar el portal.
**Depends on**: Phase 2
**Requirements**: ACCESOS-01, ACCESOS-02, ACCESOS-03
**Success Criteria** (what must be TRUE):
  1. La persona abre la sección "Accesos previos" desde el sidebar y ve los pasos para solicitar Cursor, con link al portal interno o ticketera correspondiente.
  2. La persona ve los pasos para solicitar acceso a GitHub, con link directo a la org de Habi.
  3. La persona usa el link directo a la ticketera de DevOps para abrir solicitudes y, además, dispara el formulario "Pedir accesos" (mismo componente que en Home) desde esta sección.
**Plans**: TBD
**UI hint**: yes

### Phase 4: Ruta de aprendizaje
**Goal**: Una persona operativa identifica qué cursos de Platzi tomar antes de tocar máquina y cuáles llevar en paralelo al desarrollo.
**Depends on**: Phase 3
**Requirements**: RUTA-01, RUTA-02
**Success Criteria** (what must be TRUE):
  1. La persona abre la sección "Ruta de aprendizaje" y ve dos bloques claramente separados: "Antes de tocar máquina" y "En paralelo al desarrollo".
  2. Cada bloque muestra cursos de Platzi con nombre, link y duración estimada, leídos de constantes en `ContenidoRuta.js`.
  3. La persona hace click en cualquier curso y abre la página del curso en Platzi.
**Plans**: TBD
**UI hint**: yes

### Phase 5: Setup del computador
**Goal**: Una persona operativa configura su computador con clasp, Git y ambientes dev/prod siguiendo el paso a paso, alineado con el skill global de Cursor para Prometeo.
**Depends on**: Phase 4
**Requirements**: SETUP-01, SETUP-02, SETUP-03, SETUP-04
**Success Criteria** (what must be TRUE):
  1. La persona sigue el paso a paso ordenado para instalar y configurar `clasp` desde cero.
  2. La persona configura Git localmente (clone, identidad, ssh) siguiendo las instrucciones de la sección.
  3. La persona crea ambientes `dev` y `prod` separados, en línea con cómo trabaja el resto del framework Prometeo.
  4. La persona copia los comandos shell directamente desde bloques de código copiables, sin tener que reescribirlos.
**Plans**: TBD
**UI hint**: yes

### Phase 6: Pre work de negocio
**Goal**: Una persona operativa redacta un PRD y autoevalúa su idea contra los 3 criterios de scope antes de empezar a construir.
**Depends on**: Phase 5
**Requirements**: PREWORK-01, PREWORK-02, PREWORK-03
**Success Criteria** (what must be TRUE):
  1. La persona obtiene la plantilla de PRD (link a Google Doc plantilla o texto inline en bloque copiable) desde la sección.
  2. La persona ve los 3 criterios de autoevaluación: (a) problema con horas medibles, (b) alcance ≤1 mes, (c) usado por >1 persona.
  3. La persona responde una pregunta concreta de auto-validación por cada criterio antes de avanzar a Construye.
**Plans**: TBD
**UI hint**: yes

### Phase 7: Construye
**Goal**: Una persona operativa entiende y aplica el ciclo Get Shit Done paso a paso para construir su primera automatización.
**Depends on**: Phase 6
**Requirements**: CONSTRUYE-01, CONSTRUYE-02
**Success Criteria** (what must be TRUE):
  1. La persona ve el ciclo Get Shit Done desplegado paso a paso, en orden secuencial.
  2. La persona entiende qué hacer en cada paso y cómo se relaciona con el siguiente (qué entrega y qué habilita).
**Plans**: TBD
**UI hint**: yes

### Phase 8: Paso a producción
**Goal**: Una persona operativa despliega su automatización a producción siguiendo lineamientos seguros de manejo de datos sensibles, API keys y anti-patrones explícitos.
**Depends on**: Phase 7
**Requirements**: PROD-01, PROD-02, PROD-03, PROD-04, PROD-05
**Success Criteria** (what must be TRUE):
  1. La persona distingue cuándo usar `dev` vs `prod` y qué cambia entre ambos ambientes.
  2. La persona ejecuta los comandos `clasp` y `git` en el orden documentado para llevar código a producción.
  3. La persona consulta y aplica el checklist de manejo de datos sensibles (qué se puede y qué NO) antes de desplegar.
  4. La persona maneja API keys correctamente vía `PropertiesService`, sin commitear secretos al repo.
  5. La persona reconoce los anti-patrones explícitos en "Qué NO hacer" (BigQuery sin permisos, datos personales en logs, etc.) y los evita.
**Plans**: TBD
**UI hint**: yes

## Progress

**Execution Order:**
Phases execute in numeric order: 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 (sin paralelización; una sección a la vez).

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Cimientos (Infraestructura y Estilo) | 0/TBD | Not started | - |
| 2. Home + CTA "Pedir accesos" | 0/TBD | Not started | - |
| 3. Accesos previos | 0/TBD | Not started | - |
| 4. Ruta de aprendizaje | 0/TBD | Not started | - |
| 5. Setup del computador | 0/TBD | Not started | - |
| 6. Pre work de negocio | 0/TBD | Not started | - |
| 7. Construye | 0/TBD | Not started | - |
| 8. Paso a producción | 0/TBD | Not started | - |
