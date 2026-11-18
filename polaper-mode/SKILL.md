---
name: polaper-mode
description: Modo de trabajo autónomo. Divide el prompt en tareas, las clasifica por tamaño y tipo, delega en subagentes lo paralelo e independiente, revisa solo cuando hace falta y no para hasta terminar o necesitar al usuario.
disable-model-invocation: true
argument-hint: [objetivo de la tarea]
allowed-tools: Read, Grep, Glob, Edit, Write, Bash, Agent
---

# polaper-mode

Objetivo: $ARGUMENTS

## Premisa: proceso proporcional
El proceso escala con la tarea, nunca al revés. Un cambio pequeño no lleva tests nuevos, ni review, ni ADR, ni subagentes. Antes de añadir un paso, preguntarse: "¿esto reduce un riesgo real de esta tarea?". Si no, saltarlo.

## 1. Entender y dividir
1. Leer el prompt y el código relevante. Resolver solo lo que se pueda deducir (código, docs, ADRs existentes).
2. Si el prompt empieza por "nueva tarea" / "new task", olvidar el playbook anterior y volver a clasificar.
3. Dividir en tareas concretas y verificables (TodoWrite / lista de tareas). Una tarea S sola no necesita lista.
4. **Dudas** (de `ask-when-unclear`): si una suposición equivocada obliga a rehacer trabajo, agrupar las dudas en una sola ronda de 1 a 4 preguntas de opción múltiple, con la recomendada primero. Si la duda tiene un valor por defecto razonable, usarlo y mencionarlo al final.

## 2. Clasificar cada tarea
Cada tarea se evalúa por separado: una petición puede mezclar una S y una L.

**Tamaño**
| | S (trivial) | M (normal) | L (grande o con riesgo) |
|---|---|---|---|
| Ejemplos | typo, texto, estilo concreto, 1 archivo, renombre local | feature o fix acotado, varios archivos de un módulo | varios módulos, modelo de datos, API pública, seguridad, migraciones |
| Verificación | comprobación mínima (compila, se ve) | tests existentes que afecten; test nuevo solo si es lógica no trivial | tests relevantes + prueba real del flujo |
| Review | no | autorrevisión del diff | review independiente (subagente o `/code-review`) |
| ADR | no | solo si hubo una decisión real | sí si hubo decisión |
| Delegar | no | solo si hay partes independientes grandes | sí, lo paralelo e independiente |

**Tipo**: elegir el playbook.

## 3. Playbooks
Pasos orientativos: saltar los que no aporten en esa tarea.

- **Investigación** (pregunta, "no toques código"): leer, buscar, responder con evidencia (rutas, líneas, salidas). Sin cambios.
- **Bug**: reproducir (o localizar con evidencia si no es reproducible) → causa raíz → fix mínimo → verificar que ya no pasa → test de regresión solo si el bug puede volver.
- **Feature**: ver qué existe y reutilizarlo → implementar lo mínimo que cumple → verificar el flujo real → tests si hay lógica no trivial.
- **Refactor**: sin cambio de comportamiento. Asegurar una red (tests existentes o comprobación antes/después) → cambios en pasos pequeños → verificar mismo resultado.
- **Decisión** (elegir librería, patrón, arquitectura): 2 o 3 opciones con trade-offs reales; si un experimento pequeño la resuelve, hacerlo. Recomendar una. Si es de producto o preferencia, preguntar. Registrar ADR.
- **Cambio visual**: concreto e inequívoco → aplicarlo directamente. Ambiguo → `visual-change-variants`: preview aislado solo del componente, con estilos reales, estado actual + 2 a 4 variantes (A, B, C…), publicar y esperar elección antes de tocar el código.
- **Grande o sin encaje**: dividir en fases, cada fase con su comprobación de "hecho", y aplicar a cada fase el playbook que le toque.

## 4. Delegar en subagentes
Delegar solo si se cumplen todas:
- la tarea es independiente (no necesita el resultado de otra en curso),
- no toca los mismos archivos que otra tarea en paralelo (si toca archivos, usar `isolation: worktree`),
- es lo bastante grande para compensar el coste de arrancar un agente sin contexto.

Al delegar, el prompt del subagente lleva: objetivo, archivos y contexto relevante, qué no tocar, cómo verificar y qué devolver (resumen corto + archivos cambiados). Las tareas pequeñas o secuenciales se hacen aquí directamente. Al volver, revisar lo que devuelve cada subagente antes de darlo por bueno.

## 5. Permisos y autonomía
- **Sin preguntar**: leer, buscar, editar, crear archivos, ejecutar build/tests/linters, commits locales en una rama de trabajo, todo lo que sea local y reversible.
- **Preguntar siempre antes de**: push, PR, deploy, publicar, borrar datos o archivos no creados en esta tarea, migraciones destructivas, cambiar configuración global, gastar dinero, enviar algo a servicios externos, decisiones de producto.
- Si el usuario no está y falta algo irreversible: dejar todo preparado, parar y decir exactamente qué falta.

## 6. Cerrar cada tarea
1. Verificar según su tamaño (tabla del paso 2). Si algo falla, arreglarlo antes de seguir; no maquillarlo.
2. Review si toca: M autorrevisa el diff (bugs, código muerto, sobreingeniería, convenciones del repo); L pasa por un review independiente y se aplican los hallazgos confirmados.
3. **ADR** (de `decision-records`): si hubo una decisión técnica, de diseño o de proceso, crear `docs/adr/<tipología>/NNNN-titulo.md` (contexto, decisión, alternativas descartadas, consecuencias) y añadirla al índice. Si el repo no tiene ADRs, preguntar antes de crear la estructura.

## 7. Bucle hasta terminar
Antes de escribir el resumen final, revisar la lista de tareas:
- Si queda algo pendiente dentro del objetivo ("falta X", "habría que Y"), **no se resume como pendiente**: se convierte en una tarea nueva y se vuelve al paso 2.
- Solo se para cuando: todo está hecho y verificado, o lo que queda necesita al usuario (decisión, acceso, acción irreversible). En ese caso, preguntar.
- Lo que está fuera del objetivo no se hace: se menciona en una línea como sugerencia.
- Si una tarea falla dos veces con el mismo enfoque, cambiar de enfoque o preguntar; no repetir en bucle.

## 8. Resumen final
Estilo `concise-output`: primero el resultado, después el detalle mínimo. Tabla si hay varias tareas:

| Tarea | Resultado | Verificación |
|---|---|---|
| … | ✅ hecho / ❌ falló / ⏸ necesita al usuario | test, comando o comprobación real |

Después, solo si aplica, en una línea cada uno: lo que salió mal o se descartó, ADRs creados, pregunta pendiente para el usuario. Sin listas de "pendientes" que el propio agente podía hacer, sin relleno y sin repetir.
