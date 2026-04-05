# /documentacion — Skill para Claude Code

Genera documentación profesional de entrega de proyectos para handoff a clientes. Detecta automáticamente el stack tecnológico y produce 5 documentos `.docx` con diseño profesional.

**Versión:** 2.0.0  
**Basado en:** [santmun/docs-entrega-skill](https://github.com/santmun/docs-entrega-skill) v1.0.0

---

## Documentos que genera

| Archivo | Descripción |
|---------|-------------|
| `README.docx` | Descripción general, stack, instalación rápida |
| `GUIA-DE-USO.docx` | Manual de usuario adaptado al tipo de cliente |
| `DOCS-TECNICA.docx` | Arquitectura, APIs, DB, decisiones técnicas |
| `DEPLOYMENT.docx` | Despliegue, CI/CD, rollback, mantenimiento |
| `ACCESOS-DEL-PROYECTO.docx` | Credenciales y accesos (confidencial) |
| `.env.example` | Variables de entorno documentadas |

---

## Instalación

```bash
# 1. Copiar la skill al directorio de comandos de Claude Code
mkdir -p ~/.claude/commands
cp SKILL.md ~/.claude/commands/documentacion.md

# 2. Instalar dependencia de Python
pip install python-docx

# 3. Reiniciar Claude Code para que cargue el nuevo comando
```

> **Nota:** La carpeta correcta es `~/.claude/commands/`. Cualquier archivo `.md` en esa carpeta queda disponible como `/nombre-del-archivo` en todos tus proyectos de Claude Code.

---

## Uso

En cualquier proyecto, ejecutar en Claude Code:

```
/documentacion
```

Claude te pedirá información básica y luego:
1. Analizará automáticamente tu proyecto (stack, APIs, DB, servicios, CI/CD)
2. Generará los 6 documentos en `docs/entrega/`
3. Convertirá los `.md` a `.docx` con diseño profesional

---

## Mejoras respecto a v1.0.0

### Nuevo documento: DEPLOYMENT.md
Guía completa de despliegue con:
- Variables de entorno por entorno
- Proceso de deploy paso a paso por plataforma
- Health checks y monitoreo
- Procedimiento de rollback
- Comandos de backup de DB

### Detección de stack ampliada
Detecta automáticamente:
- **Monorepos:** Turborepo, Nx, Lerna, pnpm workspaces
- **Más frameworks:** Astro, Remix, SvelteKit, FastAPI, Django, Laravel, Spring Boot, Rails, Go, Rust
- **ORMs:** Prisma, Drizzle, TypeORM, SQLAlchemy, Alembic
- **Testing:** Jest, Vitest, Playwright, Cypress
- **Infraestructura:** Docker, docker-compose, Terraform
- **CI/CD:** GitHub Actions, GitLab CI, Vercel, Netlify, Railway, Fly.io, Render

### Catálogo de servicios ampliado (40+ servicios)
Servicios nuevos en v2:
- Turso, MongoDB Atlas, Pinecone, Upstash
- Better Auth, Lucia
- Fly.io, Render, Google Cloud
- Mercado Pago, PayPal, Postmark, Mailgun
- Replicate, LlamaIndex, Datadog, Better Stack
- GitHub, GitLab

### Scripts Python mejorados

**md_to_docx.py v2:**
- Soporte para blockquotes con borde de acento
- Advertencias visuales (⚠️) con fondo amarillo
- Soporte para listas anidadas
- Etiquetas de lenguaje en bloques de código
- Mejor manejo de caracteres especiales

**generate_accesos.py v2:**
- 40+ servicios en el catálogo (vs 24 en v1)
- Aliases para normalizar nombres de servicios
- Badge "DOCUMENTO CONFIDENCIAL" en portada
- Sección de variables de entorno con conteo
- Confirmación de recepción con espacio para firma
- Agrupación por categorías con orden lógico

### Reglas de seguridad mejoradas
- Escaneo de secrets expuestos en el código
- Verificación del `.env.example` generado
- Lista de `[MARCADORES]` pendientes al final

---

## Opciones de personalización

| Opción | Descripción | Default |
|--------|-------------|---------|
| Color de acento | Color hex para diseño de documentos | `#00E5FF` |
| Logo | Ruta local o URL al logo de la empresa | Ninguno |
| Idioma | Español o English | Español |
| Tipo de cliente | Técnico / No técnico / Con mantenimiento | Técnico |

---

## Requisitos

- [Claude Code](https://claude.ai/code)
- Python 3.10+
- `python-docx` (`pip install python-docx`)

---

## Seguridad

Esta skill **nunca** incluye:
- API keys, tokens o passwords reales
- Valores del archivo `.env` (solo variables del `.env.example`)
- Información sensible de ningún tipo

Todos los valores confidenciales se reemplazan con `[MARCADORES]` que el desarrollador debe completar antes de entregar al cliente.

---

## Licencia

MIT
