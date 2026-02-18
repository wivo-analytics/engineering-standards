# Bloque de Metodología Wivo Analytics — Para inyectar en CLAUDE.md

> **Instrucciones**: Copiar el contenido entre las líneas `--- INICIO BLOQUE ---`
> y `--- FIN BLOQUE ---` al INICIO del CLAUDE.md de cada repositorio.
> Después del bloque, cada repo agrega sus estándares específicos de proyecto.
>
> **Para Claude Desktop / claude.ai**: Copiar este mismo bloque como
> "Project Instructions" o "Custom Instructions" del proyecto.

---

## Cómo queda el CLAUDE.md de cada repo

```
┌─────────────────────────────────────────┐
│  BLOQUE METODOLOGÍA WIVO (este bloque)  │  ← Igual en TODOS los repos
│  - Ciclo de desarrollo                  │
│  - Comportamiento de Claude             │
│  - Estándares universales               │
├─────────────────────────────────────────┤
│  ESTÁNDARES DEL PROYECTO                │  ← Específico de cada repo
│  - Stack tecnológico                    │
│  - Patrones de código                   │
│  - Estructura de directorios            │
│  - Cómo correr, testear, deployar       │
└─────────────────────────────────────────┘
```

---

--- INICIO BLOQUE METODOLOGÍA ---

```markdown
# Metodología de Desarrollo — Wivo Analytics

## IMPORTANTE: Lee esto PRIMERO antes de cualquier tarea

Este proyecto sigue la **Metodología de Desarrollo AI-First de Wivo Analytics**.
Estas instrucciones definen CÓMO trabajas en cualquier tarea, independiente de la tecnología.
Los estándares específicos del proyecto están más abajo en este mismo archivo.

---

## Ciclo de Desarrollo Obligatorio

Todo trabajo sigue 6 fases. La profundidad varía según complejidad, pero ninguna se salta.

### Fase 1: ENTENDER (antes de tocar código)
- Lee el issue/requerimiento completo
- Si el issue es vago o incompleto, pide clarificación o sugiere cómo completarlo
- Identifica qué archivos están involucrados explorando el codebase
- NO empieces a implementar sin entender el contexto

### Fase 2: PLANEAR (antes de implementar)
Para cambios triviales (typo, 1 línea): saltar al código.
Para todo lo demás:
- Propón un plan de implementación al usuario:
  - Archivos a modificar/crear
  - Enfoque paso a paso
  - Riesgos identificados
  - Tests que se necesitan
- Espera confirmación antes de implementar
- Si hay múltiples enfoques posibles, presenta las opciones con pros/contras

### Fase 3: IMPLEMENTAR (código + tests juntos)
- Sigue los estándares específicos del proyecto (sección siguiente de este archivo)
- Escribe tests para TODO código nuevo
- Sigue los patrones que ya existen en el proyecto
- Aplica la Regla Boy Scout: deja el código mejor de como lo encontraste
  - Si tocas código sin tests, agrega tests para tu cambio
  - Si ves logs de debug cerca, elimínalos
  - NO refactorees archivos enteros por tocar una línea

### Fase 4: VERIFICAR (antes de commit)
- Tests pasan
- Build/compilación exitoso
- Linter sin errores
- No hay logs de debug (console.log, print, etc.)
- No hay código comentado
- No hay secrets hardcodeados

### Fase 5: DOCUMENTAR COMMIT Y PR
- Mensaje de commit descriptivo que explica QUÉ y POR QUÉ
- Si se crea PR: descripción completa con cambios, testing, y contexto
- Referenciar el issue que se resuelve

### Fase 6: DOCUMENTAR CIERRE
Cuando se pida cerrar un issue, generar documentación de cierre:
- Causa raíz (si es bug)
- Solución implementada y por qué se eligió
- Archivos modificados
- Tests agregados
- Notas para el futuro

---

## Reglas Universales de Código (Todo Proyecto, Todo Stack)

Estas reglas aplican SIEMPRE, sin importar el lenguaje o framework:

1. **Tests para código nuevo**: Todo código nuevo tiene tests
2. **Sin logs de debug**: No console.log, print(), logger.debug en código de producción
3. **Sin código comentado**: Si no se usa, se borra. Git es el historial
4. **Sin valores mágicos**: Usar constantes con nombre descriptivo
5. **Sin secrets en código**: URLs, keys, passwords van en variables de entorno
6. **Nombres descriptivos**: Variables, funciones y archivos se auto-documentan
7. **Funciones enfocadas**: Cada función hace una sola cosa
8. **Archivos enfocados**: Si un archivo crece demasiado, dividirlo
9. **Errores se manejan**: No catch vacíos, no excepciones silenciadas
10. **Input externo se valida**: Nunca confiar en datos de fuera del sistema

---

## Cómo Comportarte en Cada Situación

### Cuando recibes un requerimiento nuevo
1. Lee todo el contexto disponible
2. Explora los archivos relacionados en el codebase
3. Propón plan antes de codear
4. Implementa solo después de confirmación

### Cuando corriges un bug
1. Entiende la causa raíz primero (no parchear síntomas)
2. Escribe un test que reproduce el bug ANTES de corregirlo
3. Corrige el bug
4. Verifica que el test ahora pasa

### Cuando tocas código existente (legacy)
- Sigue los estándares para TU código nuevo
- Mejora lo que esté cerca de tu cambio (Boy Scout Rule)
- NO refactorees todo el archivo si solo viniste a cambiar algo puntual
- Si el archivo es muy grande, extrae TU lógica nueva a un archivo/función separado

### Cuando no estás seguro
- Pregunta antes de asumir
- Presenta opciones con pros y contras
- Si hay riesgo de romper algo, advierte explícitamente

### Lo que NUNCA debes hacer
- Implementar sin entender el contexto
- Generar código sin tests
- Dejar logs de debug
- Silenciar errores
- Hardcodear configuración
- Cambiar más de lo pedido sin consultar
- Crear archivos enormes con múltiples responsabilidades

---

## Estructura de Issues (Referencia)

Cuando se te pida crear o mejorar un issue, usa estas estructuras:

### Bug
- Descripción (qué pasa vs qué debería)
- Pasos para reproducir (exactos, numerados)
- Comportamiento esperado
- Comportamiento actual
- Evidencia (screenshot, log, error)
- Entorno (proyecto, ambiente, versión)
- Impacto

### Feature
- Descripción (qué se necesita)
- Problema que resuelve (contexto de negocio)
- Comportamiento deseado (paso a paso)
- Alcance (incluye y NO incluye)
- Criterios de aceptación (verificables)

### Cierre de Issue
- Causa raíz (bugs)
- Solución implementada
- Archivos modificados
- Tests agregados
- PR relacionado
- Notas para el futuro

---

## Estándares del Proyecto (Específicos)

> Lo que sigue a continuación son los estándares ESPECÍFICOS de este proyecto.
> Las reglas de arriba (Metodología Wivo) tienen prioridad si hay conflicto.
```

--- FIN BLOQUE METODOLOGÍA ---

---

## Ejemplo: Cómo queda un CLAUDE.md completo

### Para un proyecto React (ej: Kodkod)
```
[Bloque Metodología Wivo]          ← Copiado de arriba
...
## Estándares del Proyecto
### Project Type
React 17 Web Application
### Stack
React 17, Redux 4, Saga, Material-UI 5
### Estándares de Componentes
- Max 250 líneas
- PropTypes obligatorios
- Max 3 useEffect
[...resto de estándares React...]
```

### Para un proyecto Python (ej: DAGs Airflow)
```
[Bloque Metodología Wivo]          ← Mismo bloque, exacto
...
## Estándares del Proyecto
### Project Type
Python Airflow DAGs
### Stack
Python 3.10, Apache Airflow 2.x
### Estándares de Código
- PEP 8
- Type hints obligatorios
- Docstrings en funciones públicas
- pytest para tests
[...resto de estándares Python...]
```

### Para un proyecto Node (ej: BFF Azure Functions)
```
[Bloque Metodología Wivo]          ← Mismo bloque, exacto
...
## Estándares del Proyecto
### Project Type
Node.js Azure Functions (BFF)
### Stack
Node.js 18, TypeScript, Azure Functions v4
### Estándares de Código
- TypeScript strict mode
- Zod para validación
- Jest para tests
[...resto de estándares Node...]
```

---

## Para Claude Desktop / claude.ai

1. Crear un "Proyecto" en Claude Desktop
2. En "Project Instructions" pegar el bloque de metodología
3. En "Project Knowledge" subir el CLAUDE.md del repo que se va a trabajar
4. Así Claude Desktop sigue la misma metodología que Claude Code en VS
