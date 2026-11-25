---
tags: [skill-creator]
max_turns: 20
timeout_seconds: 600
allowed_tools: [Read, Glob, Grep, Skill, Write, Edit, Bash]
---

En mi backend Go hay un bug intermitente: a veces el caché en memoria devuelve datos de otro usuario bajo carga. Sospecho de un map compartido entre goroutines en internal/cache/cache.go. Además quiero aprovechar para renombrar el paquete util a helpers y corregir un par de typos en los mensajes de log. Dame un plan.
