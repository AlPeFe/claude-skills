---
name: visual-change-variants
description: Usar cuando se pidan cambios visuales en una web o UI y la petición sea ambigua: generar un preview aislado solo del componente afectado con varias alternativas para elegir.
---

# Variantes de cambio visual

## Cuándo aplicar
- El usuario pide un cambio visual ("hazlo más moderno", "mejora el header", "cambia el estilo de las cards") y hay más de una interpretación razonable.
- NO aplicar si el cambio es concreto e inequívoco ("pon el botón en #FF0000", "padding 16px"): en ese caso, aplicar el cambio directamente.

## Qué hacer
1. **Aislar el componente.** Identificar solo la parte afectada (botón, card, header, formulario…). No renderizar la página completa ni la app entera.
2. **Extraer el contexto mínimo.** Copiar del código real los estilos relevantes: colores, tipografía, espaciados, variables CSS / tokens del design system. El preview debe parecerse al producto real, no a una maqueta genérica.
3. **Crear 2–4 enfoques distintos.** Cada uno debe representar una dirección diferente, no variaciones mínimas. Ejemplos: conservador (ajuste ligero), intermedio, atrevido.
4. **Montar un micro-artefacto HTML** autocontenido que muestre:
   - El estado **actual** como referencia.
   - Cada variante etiquetada (A, B, C…) con una línea que explique qué cambia y por qué.
   - Las variantes lado a lado o en grid; si el componente es interactivo (hover, focus, estados), que funcione en el preview.
   - Opcional: toggle claro/oscuro y ancho móvil/escritorio si es relevante.
5. **Publicarlo como artefacto** (o abrirlo en el navegador) para que el usuario lo vea.
6. **Pedir elección** ("¿A, B, C o una mezcla?") y **no tocar el código real** hasta que el usuario elija.
7. Tras la elección, aplicar el cambio al código del proyecto y, si es una decisión de diseño relevante, registrarla según la skill `decision-records`.

## Reglas
- Preview pequeño y rápido: solo el componente, sin datos ni lógica innecesaria.
- Usar los tokens/estilos existentes del proyecto en todas las variantes.
- Etiquetas claras y una sola frase de justificación por variante.
