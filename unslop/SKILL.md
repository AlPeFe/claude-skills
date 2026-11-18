---
name: unslop
description: >-
  Quitar patrones de escritura de IA de un texto en prosa (español o inglés). Usar en docs,
  READMEs, posts, emails o cualquier texto no-código que deba sonar humano. Triggers: "unslop",
  "humaniza esto", "que no parezca IA", "que suene natural", "limpia este texto", "quítale el tono de ChatGPT".
---

# Unslop

Reescribir un texto para quitar los patrones típicos de IA y darle voz humana. No aplica a código; sí a comentarios largos, docs y mensajes.

## Proceso
1. Detectar los patrones de abajo.
2. Reescribir manteniendo el significado y el tono que se busca (técnico, cercano, formal…).
3. Darle voz (siguiente sección).
4. Autoauditoría: "¿qué delata que esto lo ha escrito una IA?" Corregir lo que quede.
5. Entregar el texto final. Si el usuario lo pide, añadir una lista breve de los cambios principales.

## Darle voz
Quitar patrones es solo la mitad. Un texto aséptico y sin voz también se nota.
- **Tener opinión.** Reaccionar a los hechos en vez de listar pros y contras neutros.
- **Variar el ritmo.** Frases cortas. Luego otras más largas que se toman su tiempo.
- **Reconocer matices.** "Impresionante, pero algo inquietante" es mejor que "impresionante".
- **Usar primera persona cuando encaje.** No es poco profesional.
- **Dejar algo de imperfección.** Una estructura demasiado perfecta suena a algoritmo.
- **Ser concreto.** No "esto es preocupante", sino "da mal rollo ver agentes trabajando solos a las 3 de la mañana".

## Patrones a detectar

### Contenido
1. **Inflar la importancia**: "momento clave", "un testimonio de", "panorama en constante evolución", "sentar las bases", "huella imborrable". Quitar la grandilocuencia y contar qué pasó.
2. **Citar nombres por citar**: listar medios o empresas sin contexto. Elegir uno y decir qué dijo.
3. **Gerundios de relleno**: "…, destacando…", "…, garantizando…", "…, reflejando…", "…, fomentando…". Borrar o desarrollar con datos.
4. **Lenguaje promocional**: "vibrante", "impresionante", "revolucionario", "de renombre", "imprescindible", "en pleno corazón de". Describir de forma neutra.
5. **Atribuciones vagas**: "los expertos creen", "según diversos informes", "algunos críticos señalan". Nombrar la fuente o borrar.
6. **Retos de plantilla**: "A pesar de los desafíos, sigue prosperando". Sustituir por hechos concretos.

### Lenguaje
7. **Vocabulario de IA**. En español: "cabe destacar", "es fundamental", "crucial", "potenciar", "fomentar", "sinergia", "en el panorama actual", "adentrarse en", "un abanico de", "sin lugar a dudas", "en definitiva". En inglés: additionally, crucial, delve, enhance, fostering, landscape, pivotal, showcase, tapestry, testament, underscore, vibrant. Usar palabras normales.
8. **Evitar el verbo "ser"**: "se erige como", "funciona como", "cuenta con", "presume de". Decir "es" o "tiene".
9. **Paralelismos negativos**: "No es solo X, es Y" / "No se trata solo de X, sino de Y". Decirlo directamente.
10. **Regla de tres**: forzar ideas en grupos de tres. Usar el número que salga natural.
11. **Rotar sinónimos**: protagonista/personaje principal/figura central en un mismo párrafo. Elegir uno y repetirlo.
12. **Falsos rangos**: "desde X hasta Y" cuando X e Y no están en una escala real. Enumerar sin más.

### Estilo
13. **Rayas y guiones largos**: evitarlos. Usar puntos, comas o paréntesis.
14. **Dos puntos como muletilla**: bien antes de una lista o ejemplo, no como conector a mitad de frase.
15. **Negritas en exceso**: no poner en negrita cada nombre propio o sigla.
16. **Listas con cabecera en línea**: "**Rendimiento:** el rendimiento mejoró…". Pasarlo a prosa.
17. **Títulos En Mayúsculas Cada Palabra**: en español no se usa. Solo la primera letra.
18. **Emojis decorativos**: quitarlos de títulos y viñetas.
19. **Comillas tipográficas mezcladas**: unificar (rectas "…" en técnico, o latinas «…» si el texto ya las usa).
20. **Calcos del inglés**: "jugar un rol", "hacer sentido", "en orden a", "aplicar para", abusar de la pasiva ("fue desarrollado por"). Usar la forma natural en español.

### Artefactos de chatbot
21. **Frases de chatbot**: "¡Espero que te sirva!", "Si necesitas algo más, dímelo", "¡Claro!", "¡Por supuesto!". Quitar.
22. **Avisos de conocimiento limitado**: "Aunque los detalles específicos son limitados…". Buscar la fuente o quitar.
23. **Tono adulador**: "¡Excelente pregunta! ¡Tienes toda la razón!". Responder directamente.

### Relleno
24. **Frases de relleno**: "con el fin de" → "para"; "debido al hecho de que" → "porque"; "es importante señalar que" → borrar; "a día de hoy" → "hoy".
25. **Exceso de cautela**: "podría llegar a considerarse que posiblemente" → "puede".
26. **Conclusiones genéricas**: "El futuro se presenta prometedor". Dar planes o hechos concretos.

## Relación con otras skills
`concise-output` decide la estructura y longitud de las respuestas. Esta skill se encarga de que la prosa que quede suene humana. Se pueden aplicar juntas.

---
Adaptación al español de [unslop](https://github.com/poteto/noodle/tree/main/.agents/skills/unslop) de Lauren Tan (poteto), licencia MIT. Ver `LICENSE` en esta carpeta.
