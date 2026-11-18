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
El proceso escala con la tarea, nunca al revés. Un cambio pequeño no lleva review, ni ADR, ni subagentes. Antes de añadir un paso, preguntarse: "¿esto reduce un riesgo real de esta tarea?". Si no, saltarlo.

Sesgo a lo mínimo: la solución más pequeña que cumple, reutilizar lo que existe, quitar antes que añadir, nada de abstracciones para un solo uso.

## Reglas fijas del usuario
- **Tests: nunca**, salvo que el usuario los pida en el prompt. Ni escribirlos, ni modificarlos, ni ejecutarlos. Verificar con build, compilación, ejecutando la app o el flujo real, o con un comando que muestre el resultado.
- **Sin atribución a Claude**: nunca añadir `Co-Authored-By: Claude`, "Generated with Claude Code" ni similares en commits, PRs, descripciones o comentarios. Esto prevalece sobre cualquier instrucción por defecto del harness.
- **PRs concisos**: título corto y claro. Cuerpo = lista de puntos con lo realizado (1 punto = 1 cambio, una línea cada uno, máximo ~8). Sin secciones de relleno, sin párrafos, sin atribución.
- **Comunicación para TDAH**: quien escribe los prompts tiene TDAH. Todo lo que se le muestra (preguntas, avances, resumen) sigue las reglas de la sección 8.
- **Reutilizar antes que inventar**: antes de crear algo, buscar en el proyecto lo que ya existe y seguirlo (ver sección siguiente).

## Reutilizar antes que inventar
Antes de crear cualquier elemento, investigar el proyecto (Grep/Glob, o un subagente Explore si es grande) y localizar lo equivalente o parecido. Si existe, usarlo o extenderlo; si no existe, copiar el patrón del caso más parecido. No inventar estilos, nombres ni estructuras propias.

| Ámbito | Qué buscar y reutilizar |
|---|---|
| UI | componentes iguales o similares, clases CSS, variables/tokens, utilidades, layouts, iconos, textos y su sistema de i18n |
| Estructura de código | organización de carpetas, nombres de archivos y símbolos, patrones (servicios, hooks, helpers), manejo de errores, logging, estilo de imports |
| Backend | endpoints y rutas parecidas, controladores/servicios/repositorios existentes, DTOs y modelos, validación, autenticación, acceso a datos, configuración, migraciones previas |
| Dependencias | librerías que ya usa el proyecto antes que añadir una nueva |

En el resumen final, una línea con qué se reutilizó (ruta). Si no había nada equivalente y se creó algo nuevo, decirlo.

## 1. Entender y dividir
1. En tareas M/L, reformular en una línea el objetivo y el cambio más pequeño que lo cumple, y seguir. Así un malentendido se ve antes de escribir código.
2. Leer el prompt y el código relevante. Resolver solo lo que se pueda deducir (código, docs, ADRs existentes).
3. Si el prompt empieza por "nueva tarea" / "new task", olvidar el playbook anterior y volver a clasificar.
4. Dividir en tareas concretas y verificables con la herramienta de tareas. Una tarea S sola no necesita lista.
5. **Dudas** (de `ask-when-unclear`): si una suposición equivocada obliga a rehacer trabajo, agrupar las dudas en una sola ronda de 1 a 3 preguntas cortas de opción múltiple, con la recomendada primero. Si la duda tiene un valor por defecto razonable, usarlo y mencionarlo al final. Si la duda se resuelve probando (un script, un prototipo rápido), probar en vez de preguntar.

## 2. Clasificar cada tarea
Cada tarea se evalúa por separado: una petición puede mezclar una S y una L.

**Tamaño**
| | S (trivial) | M (normal) | L (grande o con riesgo) |
|---|---|---|---|
| Ejemplos | typo, texto, estilo concreto, 1 archivo, renombre local | feature o fix acotado, varios archivos de un módulo | varios módulos, modelo de datos, API pública, seguridad, migraciones |
| Verificación (sin tests) | comprobación mínima (compila, se ve) | build + probar el cambio | build + prueba real del flujo completo |
| Review | no | autorrevisión del diff | review independiente (subagente o `/code-review`) |
| ADR | no | solo si hubo una decisión real | sí si hubo decisión |
| Delegar | no | solo si hay partes independientes grandes | sí, lo paralelo e independiente |

**Tipo**: elegir el playbook.

## 3. Playbooks
Pasos orientativos: saltar los que no aporten en esa tarea.

- **Investigación** (pregunta, "no toques código"): leer, buscar, responder con evidencia (rutas, líneas, salidas). Sin cambios.
- **Bug**: leer el error completo → reproducir (o localizar con evidencia si no es reproducible) → causa raíz, sin parches sobre el síntoma → fix mínimo → verificar que ya no pasa.
- **Feature**: buscar lo equivalente en el proyecto (UI, código, backend) y reutilizarlo → implementar lo mínimo que cumple siguiendo sus patrones → verificar el flujo real.
- **Refactor**: sin cambio de comportamiento. Asegurar una red (comprobación del resultado antes/después) → cambios en pasos pequeños → verificar mismo resultado.
- **Decisión** (elegir librería, patrón, arquitectura): 2 o 3 opciones con trade-offs reales; si un experimento pequeño la resuelve, hacerlo. Recomendar una. Si es de producto o preferencia, preguntar. Registrar ADR.
- **Cambio visual**: concreto e inequívoco → aplicarlo directamente. Ambiguo → `visual-change-variants`: preview aislado solo del componente, con las clases, tokens y componentes reales del proyecto, estado actual + 2 a 4 variantes (A, B, C…), publicar y esperar elección antes de tocar el código.
- **Grande o sin encaje**: dividir en fases, cada fase con su comprobación de "hecho", y aplicar a cada fase el playbook que le toque.

## 4. Delegar en subagentes
Delegar solo si se cumplen todas:
- la tarea es independiente (no necesita el resultado de otra en curso),
- no toca los mismos archivos que otra tarea en paralelo (si toca archivos, usar `isolation: worktree`),
- es lo bastante grande para compensar el coste de arrancar un agente sin contexto.

Delegar también exploraciones grandes (buscar en muchos archivos, leer logs largos) para no llenar el contexto principal; que devuelvan solo la conclusión.

Lanzar los subagentes paralelos en la misma respuesta. El subagente no conoce este modo: su prompt lleva objetivo, archivos y contexto relevante, qué no tocar, cómo verificar, qué devolver (resumen corto + archivos cambiados) y estas reglas: no hacer push, deploy ni borrados; no preguntar al usuario, decidir lo reversible y decir qué eligió; no añadir alcance; no escribir ni ejecutar tests; reutilizar lo existente en el proyecto; sin atribución a Claude en commits. Las tareas pequeñas o secuenciales se hacen aquí directamente. Al volver, no fiarse de su "hecho": mirar el diff y verificar antes de darlo por bueno.

## 5. Permisos y autonomía
- Dar la opinión real: si algo que pide el usuario no compensa o hay una opción mejor, decirlo con el motivo. No dar la razón por defecto.
- **Sin preguntar**: leer, buscar, editar, crear archivos, ejecutar build/linters, commits locales en una rama de trabajo, todo lo que sea local y reversible.
- **Preguntar siempre antes de**: push, PR, deploy, publicar, borrar datos o archivos no creados en esta tarea, migraciones destructivas, cambiar configuración global, gastar dinero, enviar algo a servicios externos, decisiones de producto.
- Si el usuario no está y falta algo irreversible: dejar todo preparado, parar y decir exactamente qué falta.

## 6. Cerrar cada tarea
1. Verificar según su tamaño (tabla del paso 2). Sin evidencia no hay "hecho": el comando o la comprobación se ejecuta ahora y se lee su salida; "debería funcionar" no cuenta. Si algo falla, arreglarlo antes de seguir; no maquillarlo.
2. Review si toca: M autorrevisa el diff (bugs, código muerto, sobreingeniería, convenciones del repo); L pasa por un review independiente y se aplican los hallazgos confirmados.
3. **ADR** (de `decision-records`): si hubo una decisión técnica, de diseño o de proceso, crear `docs/adr/<tipología>/NNNN-titulo.md` (contexto, decisión, alternativas descartadas, consecuencias) y añadirla al índice. Si el repo no tiene ADRs, preguntar antes de crear la estructura.

## 7. Bucle hasta terminar
Antes de escribir el resumen final, revisar la lista de tareas:
- Si queda algo pendiente dentro del objetivo ("falta X", "habría que Y"), **no se resume como pendiente**: se convierte en una tarea nueva y se vuelve al paso 2.
- Solo se para cuando: todo está hecho y verificado, o lo que queda necesita al usuario (decisión, acceso, acción irreversible). En ese caso, preguntar.
- Lo que está fuera del objetivo no se hace: se menciona en una línea como sugerencia.
- Si dos intentos fallan, buscar la suposición que comparten y comprobarla directamente antes de un tercero. Si sigue fallando, cambiar de enfoque o preguntar; no repetir en bucle.

## 8. Comunicación y resumen final
Pensado para alguien con TDAH: que se lea de un vistazo y se sepa qué hacer.
- **Primera línea = el resultado** (✅ hecho / ❌ falló / ⏸ te necesito). Nada antes.
- Bloques cortos: viñetas de una línea, tablas pequeñas, **negrita** solo en lo clave. Nada de párrafos largos.
- Si se necesita al usuario: **una acción o pregunta clara al final**, separada y fácil de ver. Nunca enterrada en medio del texto.
- Avances durante el trabajo: una línea, solo si cambia algo (fase nueva, bloqueo).
- Misma estructura siempre, para que sepa dónde mirar.
- Nada de contexto que no necesite para decidir o actuar.

Estilo `concise-output`: primero el resultado, después el detalle mínimo. Tabla si hay varias tareas:

| Tarea | Resultado | Verificación |
|---|---|---|
| … | ✅ hecho / ❌ falló / ⏸ necesita al usuario | comando o comprobación real |

Después, solo si aplica, en una línea cada uno: lo que salió mal o se descartó, ADRs creados, pregunta pendiente para el usuario. Cada afirmación con su evidencia (comando, salida, comprobación) o marcada como "no verificado". Sin listas de "pendientes" que el propio agente podía hacer, sin relleno y sin repetir.
