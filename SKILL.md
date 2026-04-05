---
name: documentacion
description: "Genera documentación profesional de entrega de proyectos para handoff a clientes. Crea README.md, GUIA-DE-USO.md, DOCS-TECNICA.md, DEPLOYMENT.md, ACCESOS.md y los convierte a Word (.docx) con diseño profesional. Detecta automáticamente el stack tecnológico, variables de entorno, rutas API, CI/CD, Docker, monorepos y servicios en la nube."
metadata:
  version: 2.0.0
  author: joedcha
  based_on: santmun/docs-entrega-skill v1.0.0
---

# Skill: /documentacion

Genera un paquete completo de documentación profesional para entrega de proyectos al cliente.

## Documentos que genera

| Archivo | Descripción |
|---------|-------------|
| `README.md` | Descripción general, tecnologías, instalación rápida |
| `GUIA-DE-USO.md` | Manual de usuario adaptado al tipo de cliente |
| `DOCS-TECNICA.md` | Arquitectura, APIs, base de datos, decisiones técnicas |
| `DEPLOYMENT.md` | Guía de despliegue, CI/CD, rollback, variables de entorno |
| `ACCESOS.md` | Plantilla confidencial de credenciales y accesos |
| `.env.example` | Variables de entorno documentadas y seguras |

Los `.md` se convierten a `.docx` con diseño profesional (portada, colores de marca, tablas estilizadas).

---

## PASO 1 — Recopilar información del proyecto

Solicitar al usuario (puede presionar Enter para omitir campos opcionales):

```
Nombre del proyecto: 
Cliente / empresa: 
Tipo de cliente: [1] Técnico  [2] No técnico  [3] Con mantenimiento
URL de producción (opcional): 
Nombre del desarrollador/agencia: 
Color de acento hex (opcional, default #00E5FF): 
Logo (ruta local o URL, opcional): 
Idioma de los documentos: [1] Español (default)  [2] English
```

Si el usuario no proporciona `color de acento`, usar `#00E5FF`.
Si no especifica idioma, usar **español**.

Guardar estas respuestas para usarlas en todos los documentos.

---

## PASO 2 — Análisis automático del proyecto

Leer y analizar los siguientes archivos/patrones **en el directorio actual** (no inventar, solo leer lo que existe):

### Stack y dependencias
- `package.json` → framework (Next.js, Nuxt, SvelteKit, Astro, Remix, Express, Fastify, NestJS), dependencias, scripts
- `requirements.txt` / `pyproject.toml` / `Pipfile` → stack Python (Django, FastAPI, Flask, SQLAlchemy)
- `composer.json` → stack PHP (Laravel, Symfony)
- `go.mod` → stack Go
- `Cargo.toml` → stack Rust
- `pom.xml` / `build.gradle` → stack Java/Kotlin (Spring Boot)
- `Gemfile` → stack Ruby (Rails)

### Base de datos y ORM
- `prisma/schema.prisma` → modelos de datos, relaciones, proveedor de DB
- `drizzle.config.*` → configuración Drizzle ORM
- `typeorm.config.*` → configuración TypeORM
- `alembic/` → migraciones Python
- `migrations/` → migraciones de DB

### Variables de entorno
- `.env.example` / `.env.local.example` → variables documentadas
- `.env` (SOLO para detectar keys presentes, **nunca copiar valores reales**)
- Buscar `process.env.` en el código para detectar variables no documentadas

### APIs y rutas
- `src/app/api/` (Next.js App Router) → listar rutas API detectadas
- `src/pages/api/` (Next.js Pages Router) → listar rutas
- `routes/` o `src/routes/` → rutas del backend
- Buscar patrones `router.get/post/put/delete/patch` en archivos `.js/.ts`
- `openapi.yaml` / `swagger.json` → spec de API existente

### Infraestructura y despliegue
- `Dockerfile` / `docker-compose.yml` / `docker-compose.yaml` → configuración Docker
- `.github/workflows/` → workflows CI/CD (GitHub Actions)
- `.gitlab-ci.yml` → CI/CD GitLab
- `vercel.json` → configuración Vercel
- `netlify.toml` → configuración Netlify
- `railway.toml` → configuración Railway
- `fly.toml` → configuración Fly.io
- `render.yaml` → configuración Render
- `terraform/` → infraestructura como código

### Monorepo
- `turbo.json` → Turborepo
- `nx.json` → Nx
- `lerna.json` → Lerna
- `pnpm-workspace.yaml` / `packages/` → workspace pnpm/npm

### Autenticación y servicios
- Buscar imports de: `clerk`, `next-auth`, `auth.js`, `better-auth`, `supabase`, `firebase`, `lucia`, `passport`
- Buscar imports de: `stripe`, `openai`, `anthropic`, `resend`, `sendgrid`, `twilio`, `uploadthing`, `cloudinary`
- Buscar imports de: `sentry`, `posthog`, `datadog`, `logtail`

### Testing
- `jest.config.*` / `vitest.config.*` → tests unitarios
- `playwright.config.*` / `cypress.config.*` → tests E2E
- `*.test.ts` / `*.spec.ts` → detectar cobertura de tests

Construir un **mapa completo del stack** con todo lo encontrado antes de generar documentos.

---

## PASO 3 — Generar los documentos Markdown

Crear el directorio `docs/entrega/` y generar estos 6 archivos:

### 3.1 — README.md

```markdown
# [NOMBRE DEL PROYECTO]

> [Descripción de 2-3 líneas: qué hace el proyecto, para quién, qué problema resuelve]

**Cliente:** [NOMBRE CLIENTE]
**Versión:** 1.0.0
**Fecha de entrega:** [FECHA ACTUAL]
**Desarrollado por:** [NOMBRE DESARROLLADOR]

---

## Stack tecnológico

| Capa | Tecnología |
|------|-----------|
[Completar con stack detectado automáticamente]

## Requisitos previos

[Listar según stack detectado: Node.js versión, Python, Docker, etc.]

## Instalación rápida

```bash
# Clonar repositorio
git clone [URL_REPO]
cd [nombre-proyecto]

# Instalar dependencias
[npm install / pip install -r requirements.txt / etc según stack]

# Configurar variables de entorno
cp .env.example .env
# Editar .env con tus valores

# [Comando de setup de DB si aplica]
[npx prisma migrate dev / python manage.py migrate / etc]

# Iniciar en desarrollo
[npm run dev / python manage.py runserver / etc]
```

## URLs importantes

| Entorno | URL |
|---------|-----|
| Producción | [URL_PRODUCCION] |
| Desarrollo | http://localhost:[PUERTO] |
[Agregar staging si existe]

## Estructura del proyecto

```
[Mostrar árbol de directorios principales detectado]
```

## Scripts disponibles

[Listar scripts del package.json o Makefile detectados, con descripción de cada uno]

## Contacto y soporte

**Desarrollador:** [NOMBRE]
**Email:** [EMAIL]
[Incluir otros canales si fueron proporcionados]
```

---

### 3.2 — GUIA-DE-USO.md

Adaptar el nivel de detalle según tipo de cliente:

**Cliente técnico:** incluir comandos CLI, mencionar variables de entorno, describir arquitectura básica.
**Cliente no técnico:** usar lenguaje llano, capturas de pantalla recomendadas (marcadas con `[CAPTURA: descripción]`), explicar flujos con pasos numerados.
**Con mantenimiento:** incluir todo lo anterior más sección de monitoreo, logs, alertas y procedimientos de escalado.

```markdown
# Guía de Uso — [NOMBRE DEL PROYECTO]

**Para:** [NOMBRE CLIENTE]
**Fecha:** [FECHA ACTUAL]

---

## Acceso al sistema

[Instrucciones de login adaptadas al tipo de cliente]

## Funcionalidades principales

[Para cada módulo/funcionalidad detectada en el código:]

### [Nombre del módulo]

[Descripción de qué hace]

[Si cliente no técnico: pasos numerados con lenguaje llano]
[Si cliente técnico: puede incluir ejemplos de API calls]

[CAPTURA: descripción de la pantalla a incluir]

## Preguntas frecuentes

[Anticipar 3-5 preguntas comunes según el tipo de proyecto]

## ¿Qué hacer si algo falla?

[Procedimiento de reporte de incidencias]
[Si con mantenimiento: incluir SLA y canales de contacto]
```

---

### 3.3 — DOCS-TECNICA.md

```markdown
# Documentación Técnica — [NOMBRE DEL PROYECTO]

**Versión:** 1.0.0
**Fecha:** [FECHA ACTUAL]
**Autor:** [NOMBRE DESARROLLADOR]

---

## Arquitectura general

[Diagrama Mermaid generado automáticamente según stack detectado]

```mermaid
graph TD
[Generar diagrama real basado en el stack: frontend → backend → DB → servicios externos]
```

## Stack tecnológico detallado

[Tabla completa con versiones detectadas de cada dependencia principal]

## Base de datos

[Si se detectó Prisma/Drizzle/TypeORM/etc:]

### Modelos principales

[Listar modelos detectados con sus campos clave]

### Relaciones

[Describir relaciones detectadas en el schema]

### Migraciones

```bash
[Comandos de migración según ORM detectado]
```

## API — Endpoints

[Tabla de todos los endpoints detectados:]

| Método | Ruta | Descripción | Auth requerida |
|--------|------|-------------|---------------|
[Completar con rutas detectadas automáticamente]

## Variables de entorno

| Variable | Requerida | Descripción | Ejemplo |
|----------|-----------|-------------|---------|
[Completar con variables detectadas en .env.example y process.env]

## Autenticación y autorización

[Si se detectó sistema de auth: describir flujo, roles, tokens]

## Servicios externos integrados

[Para cada servicio detectado:]

### [Nombre del servicio]
- **Propósito:** [qué hace en el proyecto]
- **Variables de entorno requeridas:** `[VAR_NAME]`
- **Dashboard:** [URL del dashboard del servicio]

## Infraestructura y despliegue

[Si se detectó Docker:]
### Docker

```bash
# Construir imagen
docker build -t [nombre-proyecto] .

# Ejecutar con compose
docker-compose up -d
```

[Si se detectó CI/CD: describir pipeline]
### CI/CD

[Descripción del pipeline detectado en .github/workflows o equivalente]

## Testing

[Si se detectaron tests:]

| Tipo | Framework | Comando |
|------|-----------|---------|
[Completar según detección]

## Decisiones técnicas importantes

[Documentar las 3-5 decisiones arquitectónicas más relevantes detectadas en el código]

## Limitaciones conocidas y deuda técnica

[Si se detectan TODOs, FIXMEs, o patrones problemáticos: documentarlos aquí]
```

---

### 3.4 — DEPLOYMENT.md

```markdown
# Guía de Despliegue — [NOMBRE DEL PROYECTO]

**Versión:** 1.0.0
**Fecha:** [FECHA ACTUAL]
**Plataforma de despliegue:** [Detectada automáticamente]

---

## Requisitos de infraestructura

[Listar según stack detectado: Node.js version, RAM mínima, etc.]

## Variables de entorno de producción

⚠️ **Nunca commitear valores reales al repositorio.**

| Variable | Dónde configurar | Descripción |
|----------|-----------------|-------------|
[Completar con todas las variables detectadas]

## Proceso de despliegue

[Adaptar según plataforma detectada:]

### Despliegue en [PLATAFORMA DETECTADA]

```bash
[Comandos específicos de la plataforma]
```

### Pasos manuales post-despliegue

1. [Paso 1]
2. [Paso 2]
[Incluir: migraciones de DB, configuración de dominios, SSL, etc.]

## Health checks

[Si se detectan endpoints de health: documentarlos]
[Indicar qué monitorear: uptime, response time, error rate]

## Procedimiento de rollback

```bash
[Comandos o pasos para revertir a versión anterior]
```

## Actualizaciones y mantenimiento

### Actualizar dependencias

```bash
[Comandos según package manager detectado]
```

### Backup de base de datos

```bash
[Comandos según DB detectada]
```

## Solución de problemas comunes

[Basado en el stack detectado, anticipar errores comunes:]

### Error: [Nombre del error]
**Causa:** [Descripción]
**Solución:** [Pasos]
```

---

### 3.5 — ACCESOS.md

⚠️ Este documento es **CONFIDENCIAL**. Generar con valores en `[MARCADORES]` vacíos para que el cliente complete.

```markdown
# Accesos del Proyecto — [NOMBRE DEL PROYECTO]

**DOCUMENTO CONFIDENCIAL**
**Cliente:** [NOMBRE CLIENTE]
**Fecha de entrega:** [FECHA ACTUAL]
**Preparado por:** [NOMBRE DESARROLLADOR]

---

## ⚠️ Aviso de seguridad

Este documento contiene credenciales de acceso confidenciales.
- No compartir por canales no seguros (email sin cifrar, Slack, WhatsApp)
- Guardar en un gestor de contraseñas (1Password, Bitwarden, Dashlane)
- Cambiar todas las contraseñas dentro de los primeros 7 días
- Revocar accesos del desarrollador una vez recibidas las credenciales

---

## Aplicación web

| Campo | Valor |
|-------|-------|
| URL de producción | [URL_PRODUCCION] |
| Usuario administrador | [EMAIL_ADMIN] |
| Contraseña inicial | [CAMBIAR_INMEDIATAMENTE] |

[Para cada servicio detectado en el proyecto, incluir sección:]

## [Nombre del servicio]

| Campo | Valor |
|-------|-------|
| Dashboard | [URL_DASHBOARD] |
| Email/Usuario | [A_COMPLETAR] |
| Contraseña / API Key | [A_COMPLETAR] |
| Variables de entorno | `[VAR_NAME_1]`, `[VAR_NAME_2]` |
| Notas | [A_COMPLETAR] |

---

## Repositorio de código

| Campo | Valor |
|-------|-------|
| Plataforma | [GitHub / GitLab / Bitbucket] |
| URL | [URL_REPO] |
| Branch principal | main / master |
| Acceso otorgado a | [EMAIL_CLIENTE] |

---

## Variables de entorno (.env de producción)

El archivo `.env` de producción está guardado en:
`[INDICAR UBICACIÓN SEGURA: Vault, 1Password, etc.]`

---

## Confirmación de recepción

El cliente confirma haber recibido todos los accesos listados:

**Nombre:** ________________________
**Firma:** ________________________
**Fecha:** ________________________
```

---

### 3.6 — .env.example

Generar basado en **todas** las variables detectadas en el proyecto. 

Reglas:
- NUNCA incluir valores reales
- Agrupar por categoría con comentarios
- Indicar si cada variable es requerida u opcional
- Incluir ejemplos de formato donde sea útil

```bash
# ============================================
# [NOMBRE DEL PROYECTO] — Variables de entorno
# ============================================
# Copia este archivo como .env y completa los valores
# NUNCA commitear el archivo .env al repositorio

# --- Aplicación ---
NODE_ENV=development
PORT=3000
[APP_URL]=http://localhost:3000

[Continuar con todas las variables agrupadas por servicio]
```

---

## PASO 4 — Escribir los archivos

Crear el directorio `docs/entrega/` si no existe y escribir los 6 archivos generados:

```
docs/entrega/
├── README.md
├── GUIA-DE-USO.md
├── DOCS-TECNICA.md
├── DEPLOYMENT.md
├── ACCESOS.md
└── .env.example
```

Confirmar al usuario que los archivos fueron escritos.

---

## PASO 5 — Convertir a Word (.docx)

Descargar y ejecutar los scripts de conversión:

```bash
# Descargar scripts
curl -o /tmp/md_to_docx.py https://raw.githubusercontent.com/joedcha/skill-documentacion/main/scripts/md_to_docx.py
curl -o /tmp/generate_accesos.py https://raw.githubusercontent.com/joedcha/skill-documentacion/main/scripts/generate_accesos.py

# Verificar que python-docx esté instalado
pip install python-docx --quiet 2>/dev/null || pip3 install python-docx --quiet

# Detectar servicios del proyecto para el documento de accesos
SERVICES="[Lista de servicios detectados en el PASO 2, separados por comas]"

# Convertir markdown a Word
python3 /tmp/md_to_docx.py docs/entrega \
  --color "[COLOR_ACENTO]" \
  --company "[NOMBRE_DESARROLLADOR]" \
  --project "[NOMBRE_PROYECTO]" \
  [--logo "[RUTA_LOGO]" si fue proporcionado]

# Generar documento de accesos estructurado
python3 /tmp/generate_accesos.py \
  --project "[NOMBRE_PROYECTO]" \
  --client "[NOMBRE_CLIENTE]" \
  --company "[NOMBRE_DESARROLLADOR]" \
  --color "[COLOR_ACENTO]" \
  --services "$SERVICES" \
  --output docs/entrega/ACCESOS-DEL-PROYECTO.docx
```

---

## PASO 6 — Resumen final

Mostrar resumen al usuario:

```
✅ Documentación generada exitosamente

📁 docs/entrega/
   ├── README.md          → README.docx
   ├── GUIA-DE-USO.md     → GUIA-DE-USO.docx
   ├── DOCS-TECNICA.md    → DOCS-TECNICA.docx
   ├── DEPLOYMENT.md      → DEPLOYMENT.docx
   ├── ACCESOS.md         → [ver ACCESOS-DEL-PROYECTO.docx]
   ├── .env.example
   └── ACCESOS-DEL-PROYECTO.docx  ⚠️ Compartir de forma segura

📋 Stack detectado: [Lista del stack detectado]
🔧 Servicios documentados: [Lista de servicios]

🔐 Recuerda:
   • Completar los [MARCADORES] en ACCESOS-DEL-PROYECTO.docx antes de entregar
   • Compartir el documento de accesos de forma segura (no por email sin cifrar)
   • Verificar que .env.example no contenga valores reales
```

---

## Reglas importantes

1. **Solo leer, nunca inventar:** Toda la información técnica debe provenir de los archivos reales del proyecto. Si algo no se puede detectar, usar `[A_COMPLETAR]` como marcador.

2. **Seguridad ante todo:** 
   - Nunca incluir API keys, passwords, tokens o secrets reales en ningún documento
   - Escanear el `.env.example` generado para verificar que no haya valores reales
   - Advertir si se detectan secrets expuestos en el código

3. **Adaptación al cliente:** 
   - **Técnico:** incluir comandos, arquitectura, decisiones técnicas detalladas
   - **No técnico:** lenguaje llano, pasos numerados, evitar jerga técnica, marcar dónde van capturas
   - **Con mantenimiento:** todo lo anterior más procedimientos de monitoreo, SLAs, escalado

4. **Detección inteligente de servicios:** Al generar el documento de accesos, incluir SOLO los servicios que realmente se detectaron en el código. No agregar servicios genéricos que no apliquen.

5. **Diagramas Mermaid:** Generar diagramas de arquitectura reales basados en el stack detectado. El diagrama debe reflejar los componentes reales del proyecto.

6. **Fechas:** Usar formato `DD de [mes] de YYYY` en español. Ejemplo: `5 de abril de 2026`.

7. **Monorepos:** Si se detecta un monorepo (Turborepo, Nx, etc.), documentar cada app/package relevante por separado en las secciones técnicas.

8. **Docker:** Si se detecta Docker, incluir comandos de build y run en DEPLOYMENT.md. Si hay docker-compose, documentar todos los servicios definidos.

9. **Siempre convertir a Word:** Los documentos `.docx` son el entregable final. Si los scripts de Python no están disponibles o fallan, mostrar instrucciones manuales para la conversión.

10. **Verificar antes de entregar:** Al final, listar cualquier `[MARCADOR]` que quedó sin completar para que el usuario los revise antes de entregar al cliente.

---

## Catálogo de servicios (para ACCESOS-DEL-PROYECTO.docx)

| Servicio | Tipo | Variables de entorno | Dashboard |
|----------|------|---------------------|-----------|
| Supabase | DB + Auth | `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY` | supabase.com |
| Neon | DB PostgreSQL | `DATABASE_URL` | neon.tech |
| PlanetScale | DB MySQL | `DATABASE_URL` | planetscale.com |
| MongoDB Atlas | DB NoSQL | `MONGODB_URI` | cloud.mongodb.com |
| Convex | DB reactiva | `CONVEX_DEPLOYMENT`, `NEXT_PUBLIC_CONVEX_URL` | convex.dev |
| Firebase | DB + Auth + Storage | `FIREBASE_API_KEY`, `FIREBASE_PROJECT_ID` | console.firebase.google.com |
| Turso | DB SQLite edge | `TURSO_DATABASE_URL`, `TURSO_AUTH_TOKEN` | turso.tech |
| Prisma Data Proxy | ORM proxy | `DATABASE_URL` (prisma://…) | prisma.io |
| Clerk | Auth | `CLERK_SECRET_KEY`, `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` | clerk.com |
| Auth0 | Auth | `AUTH0_SECRET`, `AUTH0_BASE_URL`, `AUTH0_ISSUER_BASE_URL` | auth0.com |
| Better Auth | Auth | `BETTER_AUTH_SECRET`, `BETTER_AUTH_URL` | better-auth.com |
| NextAuth / Auth.js | Auth | `NEXTAUTH_SECRET`, `NEXTAUTH_URL` | authjs.dev |
| Lucia | Auth | `DATABASE_URL` (custom) | lucia-auth.com |
| Vercel | Hosting | (desde dashboard) | vercel.com |
| Netlify | Hosting | (desde dashboard) | netlify.com |
| Railway | Hosting + DB | (desde dashboard) | railway.app |
| Render | Hosting | (desde dashboard) | render.com |
| Fly.io | Hosting | (desde dashboard) | fly.io |
| Cloudflare | DNS + CDN + Pages | `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID` | dash.cloudflare.com |
| AWS | Cloud | `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION` | console.aws.amazon.com |
| Google Cloud | Cloud | `GOOGLE_APPLICATION_CREDENTIALS` | console.cloud.google.com |
| OpenAI | IA | `OPENAI_API_KEY` | platform.openai.com |
| Anthropic | IA | `ANTHROPIC_API_KEY` | console.anthropic.com |
| Stripe | Pagos | `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` | dashboard.stripe.com |
| Mercado Pago | Pagos | `MP_ACCESS_TOKEN`, `MP_PUBLIC_KEY` | mercadopago.com.ar/developers |
| PayPal | Pagos | `PAYPAL_CLIENT_ID`, `PAYPAL_CLIENT_SECRET` | developer.paypal.com |
| Resend | Email | `RESEND_API_KEY` | resend.com |
| SendGrid | Email | `SENDGRID_API_KEY` | sendgrid.com |
| Mailgun | Email | `MAILGUN_API_KEY`, `MAILGUN_DOMAIN` | mailgun.com |
| Postmark | Email | `POSTMARK_SERVER_TOKEN` | postmarkapp.com |
| Twilio | SMS + Tel | `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN` | twilio.com |
| UploadThing | Archivos | `UPLOADTHING_SECRET`, `UPLOADTHING_APP_ID` | uploadthing.com |
| Cloudinary | Media | `CLOUDINARY_URL` | cloudinary.com |
| AWS S3 | Storage | `AWS_S3_BUCKET`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` | console.aws.amazon.com/s3 |
| Sentry | Monitoring | `SENTRY_DSN`, `NEXT_PUBLIC_SENTRY_DSN` | sentry.io |
| PostHog | Analytics | `POSTHOG_API_KEY`, `NEXT_PUBLIC_POSTHOG_KEY` | posthog.com |
| Datadog | Monitoring | `DD_API_KEY`, `DD_APP_KEY` | datadoghq.com |
| Logtail / Better Stack | Logs | `LOGTAIL_SOURCE_TOKEN` | betterstack.com |
| LlamaParse | AI Doc parsing | `LLAMA_CLOUD_API_KEY` | llamaindex.ai |
| Replicate | AI modelos | `REPLICATE_API_TOKEN` | replicate.com |
| Pinecone | Vector DB | `PINECONE_API_KEY`, `PINECONE_ENVIRONMENT` | pinecone.io |
| Upstash | Redis + Kafka | `UPSTASH_REDIS_REST_URL`, `UPSTASH_REDIS_REST_TOKEN` | upstash.com |
| GitHub | Repo + CI/CD | (OAuth o PAT) | github.com |
