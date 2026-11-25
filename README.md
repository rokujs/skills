# skills

Mis skills para Claude Code. Cada carpeta es una skill (`<nombre>/SKILL.md`).

| Skill | Qué hace |
|---|---|
| [plan-model-split](plan-model-split/SKILL.md) | Asigna modelo (Haiku, Sonnet, Opus) y esfuerzo (low, medium, high) a cada paso de un plan de implementación y, al implementarlo, delega cada paso a un subagente con ese modelo y esfuerzo. |

## Instalación en otro equipo

Clona el repo como carpeta de skills de usuario (si ya existe `~/.claude/skills`, muévela antes):

```powershell
git clone https://github.com/rokujs/skills.git "$env:USERPROFILE\.claude\skills"
```

Para actualizar:

```powershell
git -C "$env:USERPROFILE\.claude\skills" pull
```

Claude Code carga las skills al iniciar la sesión. La carpeta `evals/` de cada skill contiene casos de prueba y no hace falta para usarla.
