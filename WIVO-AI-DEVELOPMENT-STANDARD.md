# Estándar de Desarrollo de Software con IA
## Wivo Analytics — Documento Matriz

> **Versión**: 1.0.0
> **Vigencia**: Desde Q1 2026
> **Alcance**: Toda la organización, todos los proyectos, todos los stacks
> **Aprobado por**: [CEO / CTO — Pendiente firma]
> **Última revisión**: 2026-02-18

---

## 1. Propósito

Este documento define **cómo se desarrolla software en Wivo Analytics** cuando se trabaja asistido por inteligencia artificial (Claude Code, Claude Desktop, o cualquier interfaz de Claude).

No es una sugerencia. Es el estándar. Aplica a:
- Todo desarrollador, QA e integrante de producto
- Todo proyecto (Kodkod, Almiqui, CubeJS, BFF, DAGs Airflow, Alpaca, Suricata, Meerkat)
- Todo stack (JavaScript, TypeScript, Python, o cualquier otro)
- Toda interfaz de Claude (Claude Code en VS Code, Claude Desktop, claude.ai)

### Qué NO es este documento

- No es un manual de cada tecnología (eso vive en el CLAUDE.md de cada repo)
- No es un tutorial de Claude Code (eso es documentación de Anthropic)
- No reemplaza el criterio del desarrollador (la IA asiste, el humano decide)

---

## 2. Principios Fundamentales

### 2.1. La IA amplifica, no reemplaza

Claude Code es un amplificador de capacidad. Escribe código más rápido, analiza más contexto, y mantiene consistencia. Pero **toda decisión de diseño, arquitectura y negocio la toma el humano**. El desarrollador es responsable de cada línea que se mergea, la haya escrito Claude o no.

### 2.2. Contexto es rey

La calidad del output de Claude es directamente proporcional a la calidad del input que recibe. Un issue bien escrito produce código 10x mejor que una instrucción vaga. Por eso este estándar invierte fuertemente en la calidad de los inputs.

### 2.3. Código es código

No existe "código generado por IA" vs "código escrito por humano". Todo código se evalúa con los mismos estándares: legible, testeado, documentado, seguro. Si Claude genera algo que no cumple, se corrige o se descarta.

### 2.4. Documentación es producto

Los issues, PRs, comentarios de cierre y ADRs son tan importantes como el código. Son el conocimiento institucional de la organización. Se escriben para que alguien que no estuvo presente entienda qué se hizo y por qué, incluso un año después.

### 2.5. Mejora incremental

No se detiene el desarrollo para "mejorar todo". Se mejora lo que se toca, se documenta lo que se decide, y se itera. La perfección no es el objetivo; la mejora sostenida sí.

---

## 3. El Ciclo de Desarrollo

Todo trabajo de desarrollo, sin excepción, sigue estas 6 fases. La profundidad de cada fase varía según la complejidad, pero ninguna se salta.

```
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│  ISSUE  │───▶│ DISEÑO  │───▶│ CÓDIGO  │───▶│ REVIEW  │───▶│ DEPLOY  │───▶│ CIERRE  │
│         │    │         │    │         │    │         │    │         │    │         │
│ Definir │    │ Planear │    │ Implem. │    │ Validar │    │ Entregar│    │Document.│
│ el qué  │    │ el cómo │    │ + Tests │    │ calidad │    │ con CI  │    │ el por  │
│ y por   │    │         │    │         │    │         │    │         │    │ qué     │
│ qué     │    │         │    │         │    │         │    │         │    │         │
└─────────┘    └─────────┘    └─────────┘    └─────────┘    └─────────┘    └─────────┘
```

---

### 3.1. Fase ISSUE — Definir el qué y el por qué

**Objetivo**: Que cualquier persona del equipo (o Claude) entienda exactamente qué hay que hacer, por qué, y cómo se verifica que está hecho.

**Responsable**: Quien identifica la necesidad (PM, Dev, QA, cualquiera).

#### Estructura obligatoria según tipo

**Bug:**
| Campo | Obligatorio | Descripción |
|-------|:-----------:|-------------|
| Descripción | Sí | Qué pasa vs qué debería pasar |
| Pasos para reproducir | Sí | Secuencia exacta, numerada |
| Comportamiento esperado | Sí | Qué debería ocurrir |
| Comportamiento actual | Sí | Qué ocurre |
| Evidencia | Sí | Screenshot, log, video, error exacto |
| Entorno | Sí | Proyecto, ambiente, browser/versión |
| Impacto | Sí | A quién afecta y cuánto |
| Investigación inicial | Recomendado | Hipótesis, archivos sospechosos |

**Feature:**
| Campo | Obligatorio | Descripción |
|-------|:-----------:|-------------|
| Descripción | Sí | Qué se necesita |
| Problema que resuelve | Sí | Contexto de negocio |
| Comportamiento deseado | Sí | Paso a paso de cómo funciona |
| Diseño/Mockups | Recomendado | Visual de referencia |
| Alcance | Sí | Qué incluye Y qué NO incluye |
| Criterios de aceptación | Sí | Lista verificable de "listo" |
| Contexto técnico | Recomendado | Proyectos afectados, APIs, dependencias |

**Mejora técnica:**
| Campo | Obligatorio | Descripción |
|-------|:-----------:|-------------|
| Qué mejorar | Sí | Componente, módulo, patrón |
| Estado actual | Sí | Cómo funciona hoy y por qué es problema |
| Propuesta | Sí | Cómo debería quedar |
| Riesgos | Sí | Qué podría romperse |
| Plan de migración | Recomendado | Cómo hacerlo sin romper nada |

#### Cómo Claude asiste en esta fase

Cuando el issue es incompleto o nace de una conversación informal:

```
Prompt para enriquecer issue:

"Recibí este requerimiento: [pegar texto original]

Reformúlalo como un issue de [bug/feature/mejora técnica] siguiendo esta estructura:
[pegar la estructura de arriba según tipo]

Marca con [PENDIENTE] todo lo que no puedas inferir para que yo lo complete."
```

Cuando el dev encuentra un bug mientras programa:

```
"Encontré un bug: [descripción breve del problema].
Genera un bug report completo basándote en el código que estamos viendo.
Incluye archivos afectados, posible causa raíz, y pasos de reproducción."
```

#### Criterio de calidad

Un issue está listo para la fase de diseño cuando **otro miembro del equipo puede leerlo y entender qué hay que hacer sin preguntar nada**.

---

### 3.2. Fase DISEÑO — Planear el cómo

**Objetivo**: Definir el enfoque de implementación ANTES de escribir código. Detectar problemas, identificar riesgos, y alinear expectativas.

**Responsable**: El desarrollador que tomará el issue.

#### Cuándo se necesita

| Complejidad | Ejemplo | Diseño requerido |
|-------------|---------|:----------------:|
| Trivial | Typo, cambio de texto, ajuste CSS | No |
| Baja | Fix simple, cambio en 1 archivo | Breve (5 min) |
| Media | Feature nueva, cambios en 2-5 archivos | Sí (15-30 min) |
| Alta | Cambio arquitectónico, refactoring, multi-servicio | Completo (1h+) |

#### Proceso

```
1. Leer el issue completo
2. Abrir Claude en el proyecto correspondiente
3. Solicitar análisis

   Prompt:
   "Necesito implementar esto: [pegar issue o describir]

    Analiza el codebase y dame:
    1. Archivos que necesito modificar o crear
    2. Enfoque de implementación paso a paso
    3. Código existente que puedo reutilizar
    4. Riesgos técnicos y cómo mitigarlos
    5. Tests que necesitaré escribir
    6. Complejidad estimada (S/M/L/XL)"

4. Revisar la propuesta de Claude
5. Ajustar según tu criterio y conocimiento del sistema
6. Documentar el plan en el issue como comentario
```

#### Formato del plan (comentario en el issue)

```markdown
## Plan de Implementación

**Enfoque**: [1-2 líneas describiendo la estrategia]

**Archivos a modificar**:
- `ruta/archivo` → qué cambia y por qué

**Archivos a crear** (si aplica):
- `ruta/archivo` → qué contiene y por qué

**Decisiones técnicas**:
- [Decisión] → [Por qué se eligió esta opción]

**Riesgos**:
- [Riesgo] → [Mitigación]

**Tests**:
- [Qué se va a testear]

**Complejidad**: S / M / L / XL
**Estimación**: [Si se pide]
```

#### Criterio de calidad

El plan está listo cuando: puedo dárselo a otro dev (o a Claude) y puede implementar sin ambigüedades.

---

### 3.3. Fase CÓDIGO — Implementar con tests

**Objetivo**: Producir código funcional, testeado, y que cumple los estándares del proyecto.

**Responsable**: El desarrollador asignado, asistido por Claude.

#### Flujo

```
1. Crear branch desde la rama correspondiente
   Convención:  feature/issue-XX-descripcion-corta
                bugfix/issue-XX-descripcion-corta
                hotfix/issue-XX-descripcion-corta

2. Dar contexto a Claude con el plan aprobado

   Prompt:
   "Implementa lo siguiente:
    Issue: [título y número]
    Plan: [pegar plan de la fase anterior]
    Criterios de aceptación: [pegar del issue]

    Reglas:
    - Sigue los estándares de este proyecto (CLAUDE.md)
    - Incluye tests para todo código nuevo
    - No dejes console.log/print de debug
    - No comentes código, elimínalo
    - Usa constantes, no valores mágicos"

3. Revisar el output de Claude
   - ¿La lógica es correcta?
   - ¿Los tests cubren los escenarios?
   - ¿Sigue los patrones del proyecto?
   - ¿Es la solución más simple que funciona?

4. Ajustar lo que sea necesario

5. Verificar localmente
   - Tests pasan
   - Build/compilación exitosa
   - Linter sin errores
   - Funcionalidad manual verificada (si aplica)

6. Commit con mensaje descriptivo
```

#### Reglas universales de código (todo stack)

Estas reglas aplican sin importar el lenguaje o framework:

| Regla | Justificación |
|-------|---------------|
| Todo código nuevo tiene tests | Sin tests no hay garantía de que funciona |
| No hay logs de debug en producción | console.log, print(), logger.debug en prod = ruido |
| No hay código comentado | Git es el historial, no los comentarios |
| No hay valores mágicos | Usar constantes con nombre descriptivo |
| No hay URLs/secrets en el código | Usar variables de entorno |
| Nombres descriptivos | El código se lee más de lo que se escribe |
| Funciones hacen una sola cosa | Más fácil de entender, testear y mantener |
| Archivos tienen tamaño razonable | Archivo gigante = múltiples responsabilidades mezcladas |
| Errores se manejan explícitamente | No silenciar errores, no catch vacíos |
| Input externo se valida | Nunca confiar en datos que vienen de fuera |

#### Regla Boy Scout: "Dejar el código mejor de como lo encontraste"

Cuando se modifica código existente que no cumple estos estándares:

| Nivel de cambio | Acción |
|----------------|--------|
| Toco 1-2 líneas (fix puntual) | Corregir lo inmediato, limpiar logs cercanos |
| Agrego funcionalidad a archivo | Seguir estándares para MI código. Si el archivo es grande, extraer mi lógica a archivo/función aparte |
| Refactoring significativo | Seguir todos los estándares, agregar tests completos |

**Lo que NO se hace**: Refactorear un archivo entero porque le tocaste una línea. La mejora es incremental.

#### Cómo Claude asiste en esta fase

Para implementación:
```
"Implementa [X] siguiendo los patrones existentes del proyecto.
Incluye tests. Explícame las decisiones que tomes."
```

Para entender código existente antes de modificar:
```
"Antes de modificar [archivo/función], explícame:
1. Qué hace exactamente
2. De qué depende y qué depende de esto
3. Qué podría romperse si lo cambio
4. Cómo está siendo testeado actualmente (si lo está)"
```

Para tests:
```
"Escribe tests para [componente/función] que cubran:
1. Caso exitoso (happy path)
2. Casos de error (qué pasa cuando falla)
3. Casos borde (inputs vacíos, nulos, extremos)
4. Si tiene interacción con usuario, testear las interacciones"
```

---

### 3.4. Fase REVIEW — Validar calidad

**Objetivo**: Garantizar que el código cumple estándares antes de llegar a producción. Dos capas: validación automática + revisión humana.

**Responsable**: CI/CD (automático) + otro miembro del equipo (humano).

#### Capa 1: Validación automática (CI/CD)

Cada proyecto debe tener en su pipeline de CI:

| Check | Bloquea merge | Descripción |
|-------|:-------------:|-------------|
| Tests unitarios | Sí | Todos los tests pasan |
| Build/compilación | Sí | El proyecto compila sin errores |
| Linter | Sí | Sin violaciones de estilo |
| Logs de debug | Sí | No hay console.log/print de debug |
| Tests de integración | Sí (si existen) | APIs/servicios responden correctamente |
| Cobertura de tests | Advertencia | % mínimo para código nuevo |
| Tamaño de archivos | Advertencia | Archivos nuevos dentro de límites |

#### Capa 2: Revisión humana

La persona que revisa verifica lo que la máquina no puede:

**Checklist de revisión:**
```
□ ¿Resuelve el problema descrito en el issue?
□ ¿La solución es la más simple que funciona?
□ ¿Los tests prueban lo correcto (no solo cobertura artificial)?
□ ¿Hay impacto en otros servicios o funcionalidades?
□ ¿Necesita migración de datos, config, o variables de entorno?
□ ¿La descripción del PR explica qué y por qué?
□ ¿Yo entiendo este código sin que me lo expliquen?
```

#### Estructura del Pull Request

```markdown
## Descripción
[Qué cambia y por qué — 1-3 líneas]

## Issue
Closes #XX

## Cambios realizados
- [archivo] → [qué se hizo]
- [archivo] → [qué se hizo]

## Tests
- [Qué se testeó y cómo]

## Verificación
- [ ] Tests pasan localmente
- [ ] Build exitoso
- [ ] Probado manualmente (si aplica)
- [ ] Sin logs de debug

## Screenshots (si hay cambios visuales)

## Notas para el reviewer
[Decisiones tomadas, trade-offs, cosas a las que prestar atención]

## Post-merge (si aplica)
- [ ] Requiere nuevas variables de entorno
- [ ] Requiere migración de datos
- [ ] Requiere comunicación al equipo
```

#### Cómo Claude asiste en esta fase

Como autor del PR:
```
"Genera la descripción del PR para los cambios que acabamos de hacer.
Issue relacionado: #XX
Sigue la estructura estándar del equipo."
```

Como reviewer (en Claude Desktop o web):
```
"Revisa este diff y dime:
[pegar diff o describir cambios]

1. ¿Ves problemas de lógica?
2. ¿Hay edge cases sin cubrir?
3. ¿Los tests son suficientes?
4. ¿Hay algo que pueda romperse?"
```

---

### 3.5. Fase DEPLOY — Entregar con confianza

**Objetivo**: Llevar el código a producción de forma segura, automatizada y verificable.

**Responsable**: CI/CD (automático) + Dev on-call (verificación).

#### Principio

**Todo deploy pasa por CI/CD. No hay deploys manuales.** Si hoy existe un deploy manual, automatizarlo es deuda técnica prioritaria.

#### Flujo estándar

```
Merge a rama principal
    │
    ▼
CI/CD ejecuta pipeline completo
(build → tests → deploy a staging)
    │
    ▼
Verificación en staging
(dev o QA revisa que funciona)
    │
    ▼
Promoción a producción
(automática o aprobación manual según proyecto)
    │
    ▼
Verificación post-deploy (5-15 min)
    │
    ▼
Confirmación: "Deploy exitoso" o Rollback
```

#### Verificación post-deploy

Después de cada deploy, verificar:

```
□ La funcionalidad nueva/corregida funciona
□ Los flujos críticos existentes no se rompieron
□ No hay errores nuevos en logs/monitoreo
□ Performance no se degradó
□ Si hay rollback plan, está listo por si acaso
```

#### Cómo Claude asiste en esta fase

```
"Acabo de deployar cambios del issue #XX a [staging/producción].
Los cambios afectaron estos archivos: [lista]

¿Qué flujos o funcionalidades debería verificar manualmente
para confirmar que no hay regresiones?"
```

---

### 3.6. Fase CIERRE — Documentar para el futuro

**Objetivo**: Cerrar el ciclo con documentación que sirva como referencia futura. Un issue cerrado sin documentación es trabajo invisible.

**Responsable**: El desarrollador que implementó.

#### Por qué esto importa

En 6 meses alguien va a preguntar:
- "¿Por qué se hizo así?"
- "¿Qué se intentó antes?"
- "¿Dónde está el fix de ese bug?"

Si el issue está bien cerrado, la respuesta está ahí. Si no, hay que re-investigar.

#### Formato de cierre

Antes de cerrar el issue, agregar comentario:

```markdown
## Resolución

**Causa raíz** (para bugs):
[Qué causaba el problema]

**Solución implementada**:
[Qué se hizo y por qué se eligió este enfoque]

**Archivos modificados**:
- `ruta/archivo` → [qué se cambió]

**Tests agregados**:
- [Qué verifica cada test]

**PR**: #XX
**Desplegado en**: [ambiente] — [fecha]
**Verificado por**: [nombre]

**Notas**:
[Decisiones técnicas, alternativas descartadas, deuda técnica generada, lo que sea relevante para el futuro]
```

#### Cómo Claude asiste en esta fase

```
"El PR #XX fue mergeado para el issue #YY.
Genera el comentario de cierre con:
- Resumen de la causa raíz (si es bug)
- Solución implementada
- Archivos modificados
- Tests agregados
- Notas relevantes

Basándote en todo lo que trabajamos en esta sesión."
```

Claude tiene contexto de todo lo que se hizo y genera el resumen automáticamente. El dev revisa y publica.

---

## 4. Estándares de Calidad Universales

Estos estándares aplican a **todo** proyecto, independiente del stack.

### 4.1. Código

| Estándar | Descripción | Verificación |
|----------|-------------|:------------:|
| Tests para código nuevo | Todo código nuevo tiene tests que verifican su comportamiento | CI/CD |
| Sin logs de debug | No console.log, print, logger.debug en producción | CI/CD + Review |
| Sin código comentado | Si no se usa, se borra. Git es el historial | Review |
| Sin valores mágicos | Constantes con nombre descriptivo | Review |
| Sin secrets en código | Variables de entorno para URLs, keys, passwords | CI/CD + Review |
| Nombres descriptivos | Variables, funciones, archivos se auto-documentan | Review |
| Manejo de errores | Errores se capturan y manejan, nunca se silencian | Review |
| Validación de input | Datos externos se validan antes de usar | Review |
| Funciones enfocadas | Cada función hace una sola cosa | Review |
| Archivos enfocados | Cada archivo tiene una responsabilidad clara | Review |

### 4.2. Documentación

| Estándar | Descripción |
|----------|-------------|
| Issues completos | Siguen la estructura definida en Fase 1 |
| PRs descriptivos | Siguen la estructura definida en Fase 4 |
| Issues cerrados con docs | Siguen la estructura definida en Fase 6 |
| CLAUDE.md por repo | Cada proyecto tiene su CLAUDE.md actualizado |
| ADRs para decisiones importantes | Decisiones de arquitectura documentadas |

### 4.3. Proceso

| Estándar | Descripción |
|----------|-------------|
| Branch por issue | Un branch = un issue. No mezclar cambios |
| CI/CD pasa antes de merge | No se mergea código que falla en CI |
| Review antes de merge | Otro par de ojos antes de producción |
| Deploy automático | CI/CD despliega, no personas |
| Verificación post-deploy | Se confirma que funciona después de desplegar |

---

## 5. Instrucciones para Claude (Cómo debe comportarse la IA)

Este bloque va en el CLAUDE.md de cada repo y en las instrucciones de proyectos de Claude Desktop. Es lo que hace que Claude siga la metodología automáticamente.

### Instrucciones base (copiar a cada CLAUDE.md)

```markdown
## Instrucciones de Metodología (Estándar Wivo Analytics)

Cuando trabajas en este proyecto, SIEMPRE sigue este orden:

### Antes de implementar
1. Lee y entiende el requerimiento completo
2. Explora el código existente relacionado
3. Propón un plan de implementación
4. Espera aprobación del plan antes de codear

### Durante la implementación
1. Sigue los patrones existentes del proyecto
2. Escribe tests para todo código nuevo
3. No dejes console.log/print de debug
4. No comentes código — elimínalo si no se usa
5. Usa constantes para valores fijos
6. Usa variables de entorno para URLs y configuración
7. Maneja errores explícitamente
8. Valida inputs externos
9. Funciones pequeñas que hacen una sola cosa
10. Archivos enfocados — si crece mucho, dividir

### Después de implementar
1. Verifica que tests pasan
2. Verifica que build/compilación funciona
3. Genera mensaje de commit descriptivo
4. Si se pide PR, genera descripción completa
5. Si se pide cierre de issue, genera documentación de cierre

### Lo que NUNCA debes hacer
- Implementar sin entender el contexto
- Generar código sin tests
- Dejar logs de debug
- Ignorar errores (catch vacío, except: pass)
- Hardcodear URLs, keys, o configuración
- Crear archivos enormes con múltiples responsabilidades
- Cambiar más de lo que se pidió
```

---

## 6. El CLAUDE.md: Estándares por Proyecto

### Qué es

Cada repositorio tiene un archivo `CLAUDE.md` en su raíz. Claude Code lo lee automáticamente y sigue las instrucciones. Es la **adaptación** de este estándar general al contexto específico del proyecto.

### Estructura mínima obligatoria

Todo CLAUDE.md debe contener:

```
1. DESCRIPCIÓN       → Qué es el proyecto y su rol en la arquitectura
2. STACK             → Tecnologías exactas (lenguaje, framework, versiones)
3. SETUP LOCAL       → Cómo correr el proyecto en local
4. TESTS             → Cómo correr tests
5. ESTÁNDARES        → Reglas de código específicas del proyecto
6. ARQUITECTURA      → Estructura de directorios y patrones
7. DEPLOY            → Cómo se despliega
8. VARIABLES         → Variables de entorno necesarias
9. INSTRUCCIONES IA  → Bloque del punto 5 de este documento
```

### Quién lo crea y mantiene

El desarrollador principal de cada repo. Se crea una vez y se actualiza cuando hay cambios significativos.

### Cómo crearlo desde cero (con Claude)

Abrir Claude Code en el repo y ejecutar:

```
"Explora este proyecto completo y genera un CLAUDE.md que incluya:
- Descripción del proyecto
- Stack tecnológico (infiere de package.json, requirements.txt, etc.)
- Estructura de directorios principal
- Patrones de código que ya se usan
- Cómo correr el proyecto
- Cómo correr tests
- Estándares de código apropiados para este stack

IMPORTANTE: Basate en lo que ves en el código real, no inventes.
Marca con [VERIFICAR] lo que no estés seguro."
```

Revisar el output, completar lo que falta, y hacer commit.

---

## 7. Uso en Diferentes Herramientas

### Claude Code (VS Code)

- Claude lee `CLAUDE.md` automáticamente del repo
- Seguir el ciclo de 6 fases dentro de la sesión de trabajo
- Usar los prompts sugeridos en cada fase

### Claude Desktop / claude.ai

- Crear un "Proyecto" en Claude con las instrucciones del punto 5 como knowledge
- Agregar el CLAUDE.md del repo que se va a trabajar
- Útil para: análisis de diseño, review de código, generación de issues, documentación

### GitHub (Issues y PRs)

- Las estructuras de issues y PRs de este documento se pueden configurar como templates de GitHub
- Se configuran desde la UI de GitHub (Settings → Features → Issues → Set up templates) sin tocar código

---

## 8. Distribución y Adopción

### Dónde vive este documento

**Opción recomendada**: Repositorio central `wivo-analytics/engineering-standards`

```
engineering-standards/
├── README.md                          → Índice y quick start
├── WIVO-AI-DEVELOPMENT-STANDARD.md    → ESTE DOCUMENTO (la matriz)
├── templates/
│   ├── CLAUDE-TEMPLATE.md             → Template base de CLAUDE.md
│   ├── BUG-REPORT.md                  → Template de bug report
│   ├── FEATURE-REQUEST.md             → Template de feature request
│   └── PULL-REQUEST.md                → Template de PR
├── examples/
│   ├── issue-bien-escrito.md          → Ejemplo real de buen issue
│   ├── plan-de-diseno.md              → Ejemplo real de plan
│   ├── cierre-de-issue.md             → Ejemplo real de cierre
│   └── claude-md-react.md             → Ejemplo CLAUDE.md para React
│   └── claude-md-python.md            → Ejemplo CLAUDE.md para Python
│   └── claude-md-azure-functions.md   → Ejemplo CLAUDE.md para Azure Functions
└── CHANGELOG.md                       → Historial de cambios
```

**Por qué un repo central:**
- Versionado (se puede ver qué cambió y cuándo)
- Accesible para todo el equipo (clonar o leer en GitHub)
- Se puede referenciar desde cada repo individual
- Los cambios se hacen en un solo lugar y aplican a todos

### Plan de adopción (4 semanas)

#### Semana 1 — Alineación

| Día | Acción | Responsable |
|-----|--------|-------------|
| 1 | Crear repo `engineering-standards` con este documento | Tech Lead |
| 2 | Compartir link al equipo con resumen ejecutivo (sección 9) | Tech Lead |
| 3-4 | Cada persona lee el documento completo | Todos |
| 5 | Reunión de 30 min: preguntas, aclaraciones, ajustes | Todos |

**Entregable**: Equipo alineado. Dudas resueltas. Documento ajustado si hay feedback válido.

#### Semana 2 — Piloto

| Acción | Responsable |
|--------|-------------|
| Elegir 3 issues reales (1 bug, 1 feature, 1 mejora) | Tech Lead |
| Cada dev toma 1 issue y sigue el ciclo completo de 6 fases | Devs |
| QA verifica que los issues cerrados tienen documentación | QA |
| PM crea 1 issue nuevo usando la estructura | PM |

**Entregable**: 3 issues pasaron por el ciclo completo. Se identifican fricciones reales.

#### Semana 3 — Ajuste

| Acción | Responsable |
|--------|-------------|
| Retro: ¿qué funcionó? ¿qué no? ¿qué cambiamos? | Todos |
| Ajustar este documento según feedback | Tech Lead |
| Cada dev crea CLAUDE.md en su repo principal | Devs |
| Todos los issues nuevos empiezan a usar la estructura | Todos |

**Entregable**: Proceso ajustado. CLAUDE.md creados. Issues nuevos con estructura.

#### Semana 4 — Operación

| Acción | Responsable |
|--------|-------------|
| El ciclo es el estándar para todo trabajo nuevo | Todos |
| Configurar issue templates en GitHub (opcional) | Tech Lead |
| Primera medición de métricas (ver sección 10) | Tech Lead |

**Entregable**: El equipo opera con el nuevo estándar.

---

## 9. Resumen Ejecutivo

**Para compartir con el equipo:**

---

**¿Qué es esto?**

Un estándar que define cómo desarrollamos software en Wivo Analytics cuando trabajamos con IA (Claude). Define 6 fases que todo trabajo sigue: Issue → Diseño → Código → Review → Deploy → Cierre.

**¿Por qué?**

Para que el resultado de usar IA sea consistente, de alta calidad, y documentado. Que no dependa de cómo cada persona usa Claude, sino de un proceso compartido.

**¿Qué cambia en mi día a día?**

Poco. Escribes issues con más contexto (2 min extra). Planeas antes de codear (5 min extra). Cierras issues con documentación (2 min extra). A cambio: menos bugs, menos retrabajos, menos "¿qué hiciste aquí?".

**¿Es rígido?**

No. La profundidad se adapta a la complejidad. Un typo no necesita plan de diseño. Una feature grande sí.

**¿Cuándo empezamos?**

Semana 1 leemos. Semana 2 probamos con 3 issues. Semana 3 ajustamos. Semana 4 es el nuevo normal.

---

## 10. Métricas

Se miden mensualmente. No para castigar, sino para saber si el proceso está ayudando.

| Métrica | Cómo se mide | Target 3 meses |
|---------|-------------|-----------------|
| Issues con estructura completa | Revisar 10 issues/mes | 80% |
| PRs con tests incluidos | Contar PRs con tests | 70% |
| Issues cerrados con documentación | Revisar cierres | 70% |
| Bugs que regresan | Issues reabiertos | < 10% |
| Repos con CLAUDE.md | Contar repos | 100% |

### Revisión mensual (30 min)

```
1. Revisar métricas
2. ¿Qué funcionó del proceso?
3. ¿Qué no funcionó?
4. ¿Qué ajustamos?
5. Actualizar este documento si es necesario
```

---

## 11. Gobernanza

### Quién mantiene este documento

El Tech Lead o la persona designada por el CEO. Los cambios se hacen vía PR al repo `engineering-standards` y requieren al menos 1 aprobación.

### Cuándo se actualiza

- **Inmediato**: Cuando hay un problema grave con el proceso
- **Mensual**: Después de la revisión de métricas
- **Trimestral**: Revisión completa (tecnologías, prácticas, herramientas)

### Versionado

Seguir semver:
- **Major** (2.0.0): Cambio fundamental en el proceso
- **Minor** (1.1.0): Nueva sección, nuevo estándar
- **Patch** (1.0.1): Corrección, aclaración, mejora menor

---

## Historial de Versiones

| Versión | Fecha | Cambio |
|---------|-------|--------|
| 1.0.0 | 2026-02-18 | Versión inicial |

---

*Documento generado con asistencia de Claude Code.*
*Wivo Analytics — 2026*
