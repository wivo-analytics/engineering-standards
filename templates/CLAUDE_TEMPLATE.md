# CLAUDE.md - [NOMBRE_PROYECTO]

> **Template base** — Copiar a cada repo y adaptar las secciones marcadas con `[ADAPTAR]`

## Project Type
[ADAPTAR: React Web App / Node.js API / Azure Function / Python DAG / etc.]

---

## Project Overview
[ADAPTAR: Descripción de 2-3 líneas del proyecto y su rol en la arquitectura de WivoAnalytics]

### Relación con otros servicios
[ADAPTAR: Diagrama o lista de qué servicios consume y cuáles lo consumen]

---

## Tech Stack
[ADAPTAR: Lista completa del stack]

```
- Runtime: Node.js 16 / Python 3.10 / etc.
- Framework: React 17 / Express / Azure Functions / Airflow
- State: Redux + Saga / N/A
- Testing: Jest / pytest / etc.
- CI/CD: GitHub Actions → Azure SWA / Docker Hub / Azure Functions
- Database: N/A / PostgreSQL / CosmosDB / etc.
```

---

## Getting Started

### Prerequisites
[ADAPTAR]
```bash
node -v  # >= 16.13.0
npm -v   # >= 8.0.0
```

### Install & Run
[ADAPTAR]
```bash
npm install
npm start
```

### Environment Variables
[ADAPTAR: Lista de variables de entorno necesarias]
```bash
# .env.local
REACT_APP_API_URL=
REACT_APP_AUTH0_DOMAIN=
# ... etc
```

---

## Code Standards

### File Size Limits
- Components: **250 lines** max
- Hooks: **150 lines** max
- Utilities/Services: **200 lines** max
- [ADAPTAR: Límites específicos del stack, ej: Azure Functions: 200 lines max]

### Naming Conventions
- Components: `PascalCase.js`
- Utilities: `camelCase.js`
- Hooks: `useHookName.js`
- Tests: `FileName.test.js`
- [ADAPTAR: Convenciones específicas del proyecto]

### Code Cleanliness
- **NO** `console.log()` in production code
- **NO** commented-out code
- **NO** magic numbers (use named constants)
- **NO** hardcoded URLs (use environment variables)
- [ADAPTAR: Reglas adicionales]

### Error Handling
```javascript
// [ADAPTAR: Ejemplo del patrón de error handling del proyecto]
try {
  const result = await apiCall();
  return result;
} catch (error) {
  console.error('[ModuleName]', error.message);
  throw error;
}
```

---

## Architecture

### Directory Structure
[ADAPTAR: Estructura real del proyecto]
```
src/
├── ...
```

### Key Patterns
[ADAPTAR: Patrones arquitectónicos del proyecto]

---

## Testing

### Run Tests
[ADAPTAR]
```bash
npm test              # Unit tests
npm run test:coverage # With coverage
npm run cypress:open  # E2E (if applicable)
```

### Testing Standards
- **60% coverage** minimum for new code
- **100% coverage** for critical paths
- All new code MUST have tests
- [ADAPTAR: Frameworks y patrones de testing]

---

## Deployment

### Process
[ADAPTAR: Describir el proceso de deploy exacto]

```
Branch → PR → Review → Merge → [Auto deploy / Manual step]
```

### Environments
[ADAPTAR]
| Environment | URL | Deploy trigger |
|-------------|-----|---------------|
| Local | localhost:3000 | `npm start` |
| Staging | staging.example.com | PR merge to develop |
| Production | app.example.com | Tag push |

---

## Claude Development Guidelines

When working on this project, Claude MUST:

1. **Read before writing**: Always read existing code before modifying
2. **Follow patterns**: Match existing architectural patterns
3. **Test**: Write tests for new code
4. **Size limits**: Enforce file size limits
5. **Clean code**: No console.log, no commented code
6. **Ask when unsure**: If pattern unclear, ask before implementing

### Boy Scout Rule
- Fix nearby issues when touching code
- Add missing validation when editing
- Improve incrementally, don't refactor everything at once

---

## Quality Checklist

Before marking ANY task complete:
- [ ] File sizes within limits
- [ ] No console.log statements
- [ ] No commented-out code
- [ ] Tests written and passing
- [ ] Build succeeds
- [ ] Follows project patterns

---

*Last updated: [FECHA]*
*Based on WivoAnalytics CLAUDE.md template v1.0*
