# Engineering Standards — Wivo Analytics

Estándar de desarrollo de software con IA para toda la organización.

## Documentos

| Documento | Descripción | Audiencia |
|-----------|-------------|-----------|
| [WIVO-AI-DEVELOPMENT-STANDARD.md](WIVO-AI-DEVELOPMENT-STANDARD.md) | Metodología completa de desarrollo AI-First | Todo el equipo |
| [GUIA-SETUP-CLAUDE-WIVO.md](GUIA-SETUP-CLAUDE-WIVO.md) | Guía paso a paso para instalar y configurar Claude | Nuevos integrantes |

## Repositorios relacionados

| Repositorio | Descripción |
|-------------|-------------|
| [wivo-analytics/wivo-skills](https://github.com/wivo-analytics/wivo-skills) | Skills organizacionales de Wivo para Claude Code (contexto, herramientas, automatizaciones) |

## Templates

### Meta

| Template | Uso |
|----------|-----|
| [METHODOLOGY-BLOCK.md](templates/METHODOLOGY-BLOCK.md) | Bloque para inyectar en el CLAUDE.md de cada repo |
| [CLAUDE_TEMPLATE.md](templates/CLAUDE_TEMPLATE.md) | Template base de CLAUDE.md por proyecto |
| [PULL_REQUEST_TEMPLATE.md](templates/PULL_REQUEST_TEMPLATE.md) | Template de Pull Request |

### Issues — Técnicos (engineering)

Usar cuando quien reporta es parte del equipo de ingeniería y puede describir archivos, stack, errores específicos.

| Template | Uso |
|----------|-----|
| [bug_report.md](templates/bug_report.md) | Bug Report |
| [feature_request.md](templates/feature_request.md) | Feature Request |
| [technical_improvement.md](templates/technical_improvement.md) | Mejora Técnica |

### Issues — No técnicos (producto, CS, ventas, operaciones, marketing)

Usar cuando quien reporta es un colega no-eng de la organización. Evitan jerga técnica, no piden stack traces ni criterios verificables tipo "dado/cuando/entonces". El triage de ingeniería los traduce a tickets técnicos si hace falta.

| Template | Uso |
|----------|-----|
| [user_bug_report.md](templates/non-technical/user_bug_report.md) | Reporte de algo que no funciona como se esperaba |
| [business_request.md](templates/non-technical/business_request.md) | Solicitud de funcionalidad o mejora desde negocio |
| [content_task.md](templates/non-technical/content_task.md) | Cambio de texto, traducción, o asset visual |

## Quick Start

1. Lee [WIVO-AI-DEVELOPMENT-STANDARD.md](WIVO-AI-DEVELOPMENT-STANDARD.md)
2. Sigue [GUIA-SETUP-CLAUDE-WIVO.md](GUIA-SETUP-CLAUDE-WIVO.md) para configurar tu entorno
3. Instala los skills de Wivo desde [wivo-analytics/wivo-skills](https://github.com/wivo-analytics/wivo-skills)
4. Crea o actualiza el CLAUDE.md de tu repo usando los templates

## Versionado

- **Versión actual**: 1.0.0
- **Última actualización**: 2026-02-18
- Los cambios se hacen vía PR a este repo
