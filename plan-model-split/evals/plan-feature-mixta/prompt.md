---
tags: [skill-creator]
max_turns: 20
timeout_seconds: 600
allowed_tools: [Read, Glob, Grep, Skill, Write, Edit, Bash]
---

Tengo una app Android en Kotlin (Compose + Room) para apuntar gastos. Quiero añadir exportación a CSV: un botón en Ajustes, un exportador que lea todos los gastos de Room en segundo plano y escriba el CSV con el formato correcto (escapar comas y comillas, fechas ISO), compartirlo con un Intent, tests unitarios del formateador y actualizar el README. Hazme el plan de implementación antes de tocar nada.
