# Guía de Setup: Claude AI en Wivo Analytics

> **Para**: Todo el equipo de Wivo Analytics
> **Requisito**: Cuenta en el plan Claude Team de la organización
> **Versión**: 1.0.0 — 2026-02-18

---

## Qué es esto

Esta guía te lleva de cero a productivo con Claude AI siguiendo la metodología de desarrollo de Wivo Analytics. Al terminar vas a tener:

- Claude funcionando en tu IDE y/o escritorio
- La metodología del equipo cargada para que Claude la siga automáticamente
- Tu repo configurado para desarrollo AI-assisted

---

## Paso 0: Acceso al equipo

Necesitas ser invitado al **plan Claude Team** de Wivo Analytics.

1. Pide al administrador del equipo que te invite con tu email corporativo
2. Vas a recibir un email de Anthropic con la invitación
3. Acepta la invitación y crea tu cuenta (o vincula si ya tienes una)
4. Verifica que al entrar a https://claude.ai ves el workspace del equipo

**Si no tienes invitación**: Pide al Tech Lead o al administrador del plan Team.

---

## Paso 1: Instalar las herramientas

Tienes 3 formas de usar Claude. Instala las que apliquen a tu rol.

### Opción A: Claude Desktop (Todos)

La app de escritorio. Tiene 3 modos: **Chat**, **Cowork**, y **Code**.

1. Descargar desde https://claude.com/download
2. Instalar y abrir
3. Iniciar sesión con tu cuenta del plan Team
4. Verificar que ves los 3 tabs arriba: **Chat | Cowork | Code**

**¿Cuándo usar cada modo?**

| Modo | Para qué | Ejemplo |
|------|----------|---------|
| **Chat** | Preguntas, análisis, discusión, generar texto | "¿Cómo debería estructurar esta feature?" |
| **Cowork** | Trabajo con archivos locales, documentos, análisis | Organizar docs, analizar datos, crear reportes |
| **Code** | Desarrollo de software (como terminal pero visual) | Implementar features, fix bugs, escribir tests |

### Opción B: Claude Code en VS Code (Desarrolladores)

La extensión de Claude dentro de VS Code. Lee automáticamente el `CLAUDE.md` del repo.

1. Abrir VS Code
2. Ir a Extensions (Ctrl+Shift+X)
3. Buscar **"Claude Code"** (publisher: Anthropic)
4. Instalar
5. Reiniciar VS Code
6. Abrir la paleta de comandos (Ctrl+Shift+P) → "Claude: Sign In"
7. Autenticarse con tu cuenta del plan Team

**Verificar que funciona:**
- Abrir un repo que tenga `CLAUDE.md`
- Abrir Claude Code (panel lateral o Ctrl+Shift+P → "Claude: Open")
- Escribir: "¿Qué proyecto es este?"
- Claude debería responder basándose en el CLAUDE.md del repo

### Opción C: claude.ai Web (Todos)

La interfaz web. Funciona igual que Claude Desktop pero en el browser.

1. Ir a https://claude.ai
2. Iniciar sesión con tu cuenta del plan Team
3. Listo — puedes crear Proyectos igual que en Desktop

---

## Paso 1.5: Instalar los Skills de Wivo

Los skills son modos expertos que Claude puede usar con el comando `/nombre-del-skill`. Incluyen contexto de la empresa, herramientas operativas y automatizaciones específicas de Wivo.

```bash
# Clonar el repositorio de skills
git clone https://github.com/wivo-analytics/wivo-skills ~/claude-work/wivo-skills

# Instalar
cd ~/claude-work/wivo-skills
chmod +x scripts/install.sh
./scripts/install.sh

# Reiniciar Claude Code para que los skills estén disponibles
```

**Skills instalados:**

| Skill | Para qué |
|-------|----------|
| `/wivo-kb-core` | Contexto fundacional: qué es Wivo, ICP, arquitectura, competencia |
| `/wivo-kb-ops` | Contexto operativo: roadmap, métricas, decisiones |
| `/wivo-kb-voice` | Clientes, playbooks, tono de marca |
| `/wivo-kb-writer` | Escribir y editar documentos del KB |
| `/email-outlook-wivo` | Redactar y enviar correos por Outlook |
| `/azure-monitor` | Monitoreo y diagnóstico de infraestructura Azure |
| `/wivo-website` | Editar contenido del sitio web |

**Para actualizar los skills:**

```bash
cd ~/claude-work/wivo-skills
git pull
./scripts/install.sh
```

Los skills se instalan en `~/.claude/skills/` y están disponibles en Claude Code CLI, VS Code y Claude Desktop.

---

## Paso 2: Configurar la Metodología Wivo

Aquí es donde Claude empieza a seguir nuestro estándar de desarrollo.

### Para Claude Code en VS Code

**No necesitas hacer nada extra.** Claude Code lee automáticamente el archivo `CLAUDE.md` de la raíz del repo. Si el repo ya tiene el bloque de metodología Wivo en su CLAUDE.md, Claude lo sigue.

**Si tu repo NO tiene CLAUDE.md aún** → Ve al Paso 3.

### Para Claude Desktop / claude.ai (Proyectos)

Los Proyectos son espacios de trabajo con instrucciones persistentes. Claude las lee en cada conversación dentro del proyecto.

**Crear el proyecto de tu equipo/repo:**

1. Abrir Claude Desktop (o claude.ai)
2. En el sidebar izquierdo, buscar **"Projects"** → **"Create Project"**
3. Nombre del proyecto: el nombre de tu repo (ej: "Kodkod v2", "DAGs Airflow", "BFF")
4. En **"Project Instructions"** (o "Custom Instructions"), pegar el bloque de metodología:

```
Copiar desde el archivo METHODOLOGY-BLOCK.md del repo engineering-standards,
la sección entre "--- INICIO BLOQUE METODOLOGÍA ---" y "--- FIN BLOQUE METODOLOGÍA ---"
```

5. En **"Project Knowledge"** (archivos de referencia), subir:
   - El `CLAUDE.md` completo de tu repo (si existe)
   - Cualquier doc de arquitectura relevante

6. Guardar

**A partir de ahora:** Cada conversación que inicies DENTRO de ese proyecto, Claude ya conoce la metodología y los estándares de tu proyecto.

### Para Claude Desktop — modo Code

El modo Code en Desktop funciona como Claude Code. Si apuntas a un repo que tiene `CLAUDE.md`, lo lee automáticamente. No necesitas configurar nada extra.

---

## Paso 3: Configurar tu Repo (para desarrolladores)

Si tu repo NO tiene `CLAUDE.md`, necesitas crearlo. Esto es lo que hace que Claude siga la metodología cuando trabaja en tu proyecto.

### Opción rápida: Generarlo con Claude

1. Abre Claude Code en VS Code dentro de tu repo
2. Pega este prompt:

```
Explora este proyecto completo y genera un CLAUDE.md que tenga DOS secciones:

SECCIÓN 1 — Metodología Wivo Analytics:
[Pegar aquí el bloque de metodología desde METHODOLOGY-BLOCK.md]

SECCIÓN 2 — Estándares del Proyecto:
Analiza el código del repo y genera:
- Descripción del proyecto (2-3 líneas)
- Stack tecnológico (infiere de package.json, requirements.txt, etc.)
- Estructura de directorios principal
- Patrones de código que ya se usan
- Cómo correr el proyecto localmente
- Cómo correr tests
- Estándares de código apropiados para este stack
- Cómo se despliega

IMPORTANTE: Basate en lo que ves en el código real.
Marca con [VERIFICAR] lo que no estés seguro.
```

3. Revisa el output, completa lo que tenga [VERIFICAR]
4. Guarda como `CLAUDE.md` en la raíz del repo
5. Commit:
```
git add CLAUDE.md
git commit -m "docs: Add CLAUDE.md with Wivo methodology + project standards"
```

### Opción manual

1. Copia el bloque de metodología desde `METHODOLOGY-BLOCK.md`
2. Crea `CLAUDE.md` en la raíz de tu repo
3. Pega el bloque al inicio
4. Agrega debajo los estándares específicos de tu proyecto
5. Commit

---

## Paso 4: Verificar que funciona

### Test rápido (2 minutos)

Abre Claude Code en tu repo (o una conversación en el Proyecto de Claude Desktop) y escribe:

```
Necesito agregar una función que [algo simple relevante a tu proyecto].
```

**Claude debería:**
1. Primero explorar el código existente (Fase 1: ENTENDER)
2. Proponerte un plan antes de escribir código (Fase 2: PLANEAR)
3. Esperar tu confirmación
4. Implementar con tests (Fase 3: IMPLEMENTAR)

**Si Claude se lanza directo a escribir código sin planear** → el CLAUDE.md no se está leyendo. Verifica que existe en la raíz del repo y que el bloque de metodología está al inicio.

---

## Referencia Rápida: Los 3 Modos de Trabajo

### Modo 1: Desarrollo en VS Code (día a día del dev)

```
1. Abrir repo en VS Code
2. Abrir Claude Code (panel lateral)
3. Claude ya leyó CLAUDE.md → sigue la metodología
4. Trabajar normalmente: pedir análisis, implementar, testear
```

**Cuándo usar**: Implementación de features, fix de bugs, refactoring, escribir tests.

### Modo 2: Proyecto en Claude Desktop/Web (análisis, diseño, discusión)

```
1. Abrir Claude Desktop → ir al Proyecto de tu repo
2. Claude ya tiene las instrucciones del proyecto
3. Conversar sobre diseño, arquitectura, análisis
```

**Cuándo usar**: Planear una feature antes de codear, analizar un bug complejo, discutir arquitectura, revisar código sin estar en el IDE, crear issues, documentar.

### Modo 3: Chat rápido (consultas puntuales)

```
1. Abrir Claude Desktop o claude.ai → chat normal (sin proyecto)
2. No tiene contexto de la metodología
3. Es una conversación genérica
```

**Cuándo usar**: Preguntas generales de programación, investigar una librería, consultas que no son del proyecto.

---

## Para Nuevos Integrantes del Equipo

### Tu primer día

| Paso | Acción | Tiempo |
|------|--------|--------|
| 1 | Recibir invitación al plan Claude Team | Admin te invita |
| 2 | Instalar Claude Desktop + Claude Code en VS Code | 10 min |
| 3 | Leer el documento de metodología (`WIVO-AI-DEVELOPMENT-STANDARD.md`) | 20 min |
| 4 | Instalar los skills de Wivo (Paso 1.5) | 5 min |
| 5 | Clonar el/los repos en los que vas a trabajar | 5 min |
| 6 | Verificar que el repo tiene `CLAUDE.md` (si no, crearlo con Paso 3) | 10 min |
| 7 | Crear tu Proyecto en Claude Desktop para el repo | 5 min |
| 8 | Hacer el test rápido (Paso 4) | 2 min |
| 9 | Tomar tu primer issue y seguir el ciclo de 6 fases | — |

### Lo que necesitas saber

1. **Toda tarea sigue 6 fases**: Entender → Planear → Implementar → Verificar → Documentar commit → Documentar cierre
2. **Claude ya conoce las fases** porque están en el CLAUDE.md del repo
3. **Tú decides, Claude ejecuta**: Claude propone, tú revisas y apruebas
4. **Si Claude hace algo raro**: Probablemente falta contexto. Dale más información o pídele que lea archivos específicos
5. **Pregunta al equipo** si algo no queda claro — el proceso se mejora con feedback

### Tu primera tarea (guiada)

Pide al Tech Lead que te asigne un issue con etiqueta `good-first-issue`. Sigue estos pasos:

```
1. Lee el issue completo
2. Abre Claude Code en el repo
3. Di: "Lee el issue #XX y dime qué entiendes,
        qué archivos están involucrados,
        y propón un plan de implementación"
4. Revisa la propuesta de Claude
5. Si el plan tiene sentido, dile: "Procede con la implementación"
6. Revisa el código generado
7. Verifica que tests pasan
8. Haz commit y abre PR
9. Pide review a un colega
```

---

## Troubleshooting

### "Claude no sigue la metodología, se lanza directo a codear"
- Verificar que `CLAUDE.md` existe en la raíz del repo
- Verificar que el bloque de metodología está al INICIO del archivo
- En Claude Desktop: verificar que estás dentro de un Proyecto con las instrucciones

### "Claude Code no se conecta en VS Code"
- Verificar que la extensión está instalada y actualizada
- Ctrl+Shift+P → "Claude: Sign In" → re-autenticar
- Verificar conexión a internet

### "No veo los modos Chat/Cowork/Code en Claude Desktop"
- Actualizar Claude Desktop a la última versión
- Los 3 modos están disponibles en el plan Team

### "Claude genera código que no sigue los patrones del proyecto"
- El CLAUDE.md necesita más detalle en la sección de estándares del proyecto
- Abre Claude Code y dile: "Lee los archivos [X, Y, Z] para entender los patrones antes de implementar"

### "Mi colega de Python no sabe qué estándares poner"
- Abrir Claude Code en el repo Python
- Decirle: "Explora este proyecto y sugiere estándares de código para el CLAUDE.md basándote en lo que ves"
- Claude analizará el código existente y sugerirá estándares apropiados

---

## Resumen Visual

```
                    ┌──────────────────────────────┐
                    │  WIVO-AI-DEVELOPMENT-STANDARD │
                    │  (Documento para humanos)     │
                    │  Repo: engineering-standards   │
                    └──────────────┬───────────────┘
                                   │
                    Se extrae el bloque operativo
                                   │
              ┌────────────────────┼────────────────────┐
              ▼                    ▼                     ▼
     ┌────────────────┐  ┌─────────────────┐  ┌──────────────────┐
     │  CLAUDE.md     │  │ Claude Desktop  │  │ claude.ai Web    │
     │  en cada repo  │  │ → Proyectos     │  │ → Proyectos      │
     │                │  │ → Instructions  │  │ → Instructions   │
     │  Claude Code   │  │                 │  │                  │
     │  lo lee auto   │  │  Claude lo lee  │  │  Claude lo lee   │
     │  en VS Code    │  │  en cada chat   │  │  en cada chat    │
     └────────────────┘  │  del proyecto   │  │  del proyecto    │
                         └─────────────────┘  └──────────────────┘
              │                    │                     │
              └────────────────────┼────────────────────┘
                                   │
                                   │  + Skills disponibles en todas las superficies
                                   │
                    ┌──────────────┴───────────────┐
                    │  wivo-skills                  │
                    │  Repo: wivo-analytics/wivo-skills │
                    │  Instalados en ~/.claude/skills/  │
                    │                               │
                    │  /wivo-kb-core  /azure-monitor│
                    │  /wivo-kb-ops   /email-outlook│
                    │  /wivo-kb-voice  ...          │
                    └──────────────┬───────────────┘
                                   ▼
                    ┌──────────────────────────────┐
                    │  TODOS SIGUEN LA MISMA       │
                    │  METODOLOGÍA DE 6 FASES      │
                    │                              │
                    │  Entender → Planear →        │
                    │  Implementar → Verificar →   │
                    │  Documentar → Cerrar         │
                    └──────────────────────────────┘
```

---

## Preguntas Frecuentes

**¿Tengo que usar Claude para todo?**
No. Claude es una herramienta. Si un fix es de 1 línea y lo tienes claro, hazlo directo. La metodología de 6 fases aplica al trabajo, no a la herramienta.

**¿Puedo personalizar mis instrucciones de Claude?**
Sí. El CLAUDE.md del repo y las instrucciones del Proyecto son configurables. Lo que no cambia es el bloque de metodología Wivo que va al inicio — eso es el estándar del equipo.

**¿Qué pasa si Claude sugiere algo que no tiene sentido?**
Rechazarlo. Claude propone, tú decides. Si insiste, dale más contexto o pídele que considere [X factor].

**¿Puedo usar Claude en un repo que no tiene CLAUDE.md?**
Sí, pero Claude no tendrá contexto del proyecto ni seguirá la metodología automáticamente. Tendrías que dar instrucciones manualmente en cada conversación. Mejor crear el CLAUDE.md (10 minutos con la Opción rápida del Paso 3).

**¿El CLAUDE.md se commitea al repo?**
Sí. Es un archivo más del proyecto. Todos los devs que clonan el repo lo tienen y Claude lo lee automáticamente.

**¿Cada cuánto se actualiza el CLAUDE.md?**
Cuando hay cambios significativos en el proyecto (nuevo stack, nuevos patrones, nuevas reglas). No es necesario actualizarlo constantemente.

---

*Guía generada con asistencia de Claude Code — Wivo Analytics 2026*
