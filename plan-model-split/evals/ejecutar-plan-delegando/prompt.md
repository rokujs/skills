---
tags: [skill-creator]
max_turns: 20
timeout_seconds: 600
allowed_tools: [Read, Glob, Grep, Skill, Write, Edit, Bash]
---

Estás en una sesión con Sonnet y esfuerzo medium. Este plan ya está aprobado, ejecútalo (pero de momento NO lances subagentes ni edites código: escribe en ejecucion.md exactamente qué harías en cada paso, qué llamadas a Agent harías con qué parámetros y el brief completo de cada una).

Reparto: 1 Opus·high, 2 Sonnet·medium, 3 Haiku·low
1. Rediseñar el sistema de reintentos con backoff exponencial y jitter en src/net/retry.ts, sin romper la cancelación por AbortSignal — Modelo: Opus · Esfuerzo: high
2. Mostrar en la UI (src/ui/StatusBar.tsx) el número de reintentos en curso usando el nuevo evento onRetry — Modelo: Sonnet · Esfuerzo: medium
3. Documentar las nuevas opciones en docs/red.md — Modelo: Haiku · Esfuerzo: low
4. Ejecutar npm test y revisar los diffs — Modelo: sesión

El proyecto escribe comentarios y commits en español y su CLAUDE.md dice que el script de limpieza es npm run teardown.
