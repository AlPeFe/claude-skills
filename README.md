# Claude Skills

Skills para Claude (Claude Code / claude.ai) en español.

| Skill | Qué hace |
|---|---|
| [polaper-mode](polaper-mode/SKILL.md) | **Modo de trabajo** (`/polaper-mode <objetivo>`). Divide en tareas, clasifica por tamaño y tipo, delega lo paralelo, revisa solo cuando hace falta y sigue hasta terminar. Usa el resto de skills. |
| [ask-when-unclear](ask-when-unclear/SKILL.md) | Antes de implementar, detecta dudas de alcance, enfoque o criterios de aceptación y pregunta solo lo que no se pueda deducir. |
| [decision-records](decision-records/SKILL.md) | Al cerrar una tarea con una decisión técnica, la registra como ADR en `docs/adr/<tipología>/` con índice global. |
| [concise-output](concise-output/SKILL.md) | Respuestas al grano: primero el resultado, formato más claro (tabla, gráfico, lista o texto breve) y sin relleno. |
| [unslop](unslop/SKILL.md) | Quita patrones de escritura de IA y da voz humana a textos en prosa. Adaptada al español de [poteto/noodle](https://github.com/poteto/noodle) (MIT). |
| [visual-change-variants](visual-change-variants/SKILL.md) | Ante cambios visuales ambiguos, genera un preview aislado del componente con 2–4 variantes para elegir antes de tocar el código. |

## polaper-mode: flujo

El proceso escala con la tarea: una tarea S va directa; las L pasan por delegación, review y ADR.

```mermaid
flowchart TD
    P["/polaper-mode objetivo"] --> U{"¿Dudas que obliguen<br/>a rehacer trabajo?"}
    U -->|sí| Q["ask-when-unclear<br/>1 ronda, 1-3 preguntas"]
    U -->|no| T
    Q --> T["Dividir en tareas"]
    T --> C{"Clasificar cada tarea<br/>tamaño S / M / L + tipo"}

    C -->|Investigación| I["Leer y responder<br/>con evidencia"]
    C -->|Bug| B["Reproducir, causa raíz,<br/>fix mínimo"]
    C -->|Feature| F["Reutilizar lo existente,<br/>implementar lo mínimo"]
    C -->|Refactor| R["Red de seguridad,<br/>pasos pequeños"]
    C -->|Decisión| D["2-3 opciones,<br/>experimento, recomendar"]
    C -->|Visual| V{"¿Ambiguo?"}
    V -->|sí| VV["visual-change-variants<br/>preview A/B/C y elegir"]
    V -->|no| VA["Aplicar directo"]

    F & R & B -.->|"L: partes independientes"| SA["Subagentes en paralelo<br/>(worktree si tocan archivos)"]

    I & B & F & R & D & VV & VA & SA --> VER["Verificar según tamaño"]
    VER --> REV{"Tamaño"}
    REV -->|S| OK
    REV -->|M| SR["Autorrevisión del diff"]
    REV -->|L| CR["Review independiente"]
    SR & CR --> ADR{"¿Hubo decisión?"}
    ADR -->|sí| DR["decision-records<br/>docs/adr/tipología/"]
    ADR -->|no| UNS
    DR --> UNS{"¿Hay prosa<br/>escrita o cambiada?"}
    UNS -->|sí| UN2["unslop<br/>docs, ADR, PR, README"]
    UNS -->|no| OK
    UN2 --> OK{"¿Queda algo<br/>dentro del objetivo?"}
    OK -->|"sí, lo puedo hacer"| T
    OK -->|"necesita al usuario"| H["Preguntar"]
    OK -->|no| S["Resumen final<br/>concise-output"]
```

### Estructura del repo

```mermaid
flowchart LR
    PM["polaper-mode<br/>(orquestador)"]
    PM -->|paso 1: dudas| AWU["ask-when-unclear"]
    PM -->|cambio visual ambiguo| VCV["visual-change-variants"]
    PM -->|cierre de tarea| DR["decision-records"]
    PM -->|resumen final| CO["concise-output"]
    VCV -->|variante elegida| DR
    PM -->|prosa escrita| UN["unslop"]
    DR -->|texto del ADR| UN
```

## Instalación

**Claude Code:** copia la carpeta de la skill a `~/.claude/skills/` (global) o a `.claude/skills/` dentro de un proyecto.

```bash
git clone https://github.com/AlPeFe/claude-skills.git
cp -r claude-skills/ask-when-unclear ~/.claude/skills/
```

**claude.ai:** comprime la carpeta de la skill en un `.zip` y súbela en *Settings → Capabilities → Skills*.

Para usar `polaper-mode` instala también las demás skills (las referencia por nombre; si faltan, tiene un resumen de cada una dentro).

```bash
cp -r claude-skills/*/ ~/.claude/skills/
```
