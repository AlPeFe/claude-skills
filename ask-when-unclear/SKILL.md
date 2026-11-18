---
name: ask-when-unclear
description: Usar cuando una petición de desarrollo sea ambigua (qué construir, alcance, enfoque técnico o criterios de aceptación) y una suposición equivocada obligaría a rehacer trabajo: preguntar antes de implementar. No usar si la petición es clara.
---

# Preguntar cuando haya dudas

## Principio
Antes de escribir código, comprobar si la petición está suficientemente clara. Si una suposición equivocada obligaría a rehacer trabajo, preguntar primero.

## Checklist antes de empezar
Revisar si queda claro:
- **Qué** se quiere construir o cambiar (resultado esperado).
- **Alcance**: qué entra y qué no (¿solo este componente o toda la sección?).
- **Dónde**: archivos, módulos o servicios afectados.
- **Cómo**: si hay varias opciones técnicas con trade-offs reales (librería, patrón, estructura de datos).
- **Criterios de aceptación**: cómo sabremos que está terminado.
- **Restricciones**: compatibilidad, rendimiento, convenciones del repo, cosas que no se deben tocar.

Primero investigar lo que se pueda resolver solo (leer el código, la documentación, ADRs existentes). Preguntar solo lo que no se pueda deducir.

## Cómo preguntar
- Usar preguntas de opción múltiple (AskUserQuestion o equivalente) cuando sea posible, con la opción recomendada primero y marcada como "(Recomendado)".
- Máximo 1–4 preguntas por ronda, cada una autocontenida.
- Explicar en una línea por qué importa la duda si no es obvio.
- Agrupar las dudas en una sola ronda en vez de preguntar a goteo.

## Cuándo NO preguntar
- La petición es clara o el cambio es trivial y fácil de revertir: hacerlo directamente.
- La duda tiene un valor por defecto convencional en el repo: usarlo y mencionarlo.
- El usuario ha dicho "decide tú": decidir, y dejar constancia de la decisión y de la alternativa descartada.

## Si el usuario no está disponible
Tomar la interpretación más razonable, indicarlo al principio del resultado y continuar. Excepción: decisiones irreversibles (borrar datos, migraciones destructivas, despliegues) — preparar el trabajo y parar.
