# Claude Skills

Skills para Claude (Claude Code / claude.ai) en español.

| Skill | Qué hace |
|---|---|
| [ask-when-unclear](ask-when-unclear/SKILL.md) | Antes de implementar, detecta dudas de alcance, enfoque o criterios de aceptación y pregunta solo lo que no se pueda deducir. |
| [decision-records](decision-records/SKILL.md) | Al cerrar una tarea con una decisión técnica, la registra como ADR en `docs/adr/<tipología>/` con índice global. |
| [visual-change-variants](visual-change-variants/SKILL.md) | Ante cambios visuales ambiguos, genera un preview aislado del componente con 2–4 variantes para elegir antes de tocar el código. |

## Instalación

**Claude Code:** copia la carpeta de la skill a `~/.claude/skills/` (global) o a `.claude/skills/` dentro de un proyecto.

```bash
git clone https://github.com/AlPeFe/claude-skills.git
cp -r claude-skills/ask-when-unclear ~/.claude/skills/
```

**claude.ai:** comprime la carpeta de la skill en un `.zip` y súbela en *Settings → Capabilities → Skills*.
