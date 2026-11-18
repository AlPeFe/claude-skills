---
name: concise-output
description: Usar al redactar cualquier respuesta o resumen final para el usuario: ir al grano, resumir resultados y elegir el formato más denso y claro (tabla, gráfico, lista o texto breve). Sin divagar ni repetir.
---

# Output conciso

## Principio
El usuario quiere el resultado, no el proceso. Cada frase debe aportar información nueva; si se puede quitar sin perder nada, se quita.

## Estructura de la respuesta
1. **Primero la respuesta o el resultado**, en una o dos frases. Nada de introducciones ("Voy a…", "Buena pregunta…").
2. **Después el detalle mínimo necesario** para entenderlo o actuar.
3. **Al final, solo si aplica**: siguiente paso, bloqueo o decisión pendiente, en una línea.

## Elegir el formato
| Contenido | Formato |
|---|---|
| Comparar opciones, archivos, resultados o atributos | Tabla |
| Datos numéricos en el tiempo o entre categorías | Gráfico (herramienta de gráficos si existe; si no, tabla) |
| Flujos, arquitectura, dependencias | Diagrama breve (Mermaid o ASCII) |
| Pasos a seguir | Lista numerada |
| Puntos independientes | Viñetas cortas |
| Explicación o matiz que no cabe en lo anterior | Texto, 1–3 frases por párrafo |

No forzar el formato: una respuesta de una línea no necesita tabla ni encabezados.

## Reglas
- Resumir outputs largos (logs, tests, búsquedas): dar la conclusión y solo las líneas relevantes, no el volcado completo.
- No repetir lo que el usuario ya dijo ni lo que ya se explicó antes en la conversación.
- No narrar opciones que no se van a seguir; si hay que elegir, dar una recomendación.
- Sin relleno: sin disculpas, sin cortesías vacías, sin resumen final que repita lo anterior.
- Usar cifras y datos concretos en vez de adjetivos ("3 tests fallan" mejor que "algunos problemas").
- Rutas, comandos y nombres en `código`.
- Si algo falló o no se hizo, decirlo claramente y en una línea.

## Cuándo extenderse
Solo si el usuario pide explicación detallada, o si omitir algo haría que actúe mal (riesgos, efectos irreversibles). Aun así, ordenado y sin repetir.
