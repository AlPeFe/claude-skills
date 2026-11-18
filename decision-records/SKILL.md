---
name: decision-records
description: Usar al completar una tarea o acción que implique una decisión técnica, de arquitectura, diseño o proceso: registrarla como ADR en un archivo .md separado, organizado por tipología.
---

# Registro de decisiones (ADR) por tipología

## Cuándo aplicar
Al terminar una tarea o acción que haya implicado una decisión que alguien querría entender más adelante, por ejemplo:
- elegir una librería, patrón o estructura,
- cambiar un modelo de datos o un contrato de API,
- elegir una variante de diseño o UI,
- cambiar infraestructura, despliegue o configuración,
- adoptar o cambiar una convención de proceso o de código.

No aplicar a cambios triviales sin decisión de por medio (typos, renombres menores, bugfixes obvios).

## Estructura de carpetas
Un archivo por decisión, separado por tipología:

```
docs/adr/
  README.md              # índice de todas las decisiones
  architecture/
  frontend-ui/
  backend-api/
  data/
  infra-devops/
  security/
  process/
```

- Si el repo ya tiene una carpeta de ADRs o una taxonomía propia, usar la existente.
- Si el repo no tiene ADRs, preguntar al usuario antes de crear la estructura `docs/adr/` (y dónde ubicarla). Si no la quiere, no crear nada y resumir la decisión en la respuesta.
- Crear una carpeta de tipología nueva solo si ninguna encaja.

## Nombre del archivo
`NNNN-titulo-en-kebab-case.md`, con numeración global (no por carpeta) para que cada ADR tenga un ID único. Ejemplo: `docs/adr/frontend-ui/0007-cards-con-layout-compacto.md`.

## Plantilla
```markdown
# ADR-NNNN: Título

- **Estado:** Aceptada | Propuesta | Sustituida por ADR-XXXX | Obsoleta
- **Fecha:** AAAA-MM-DD
- **Tipología:** frontend-ui
- **Tarea relacionada:** enlace o ID

## Contexto
Qué problema o necesidad había y qué restricciones aplicaban.

## Decisión
Qué se decidió, de forma concreta.

## Alternativas consideradas
- Opción A — por qué se descartó
- Opción B — por qué se descartó

## Consecuencias
Positivas, negativas y lo que habrá que vigilar o hacer después.

## Archivos afectados
Lista de rutas principales.
```

## Pasos
1. Al cerrar la tarea, decidir si hubo una decisión registrable.
2. Si es el primer ADR del repo, preguntar antes de crear la estructura (ver arriba).
3. Elegir la tipología y el siguiente número libre (revisar `docs/adr/README.md`).
4. Crear el archivo con la plantilla, rellenada con lo que realmente ocurrió (incluidas las alternativas que se descartaron durante la conversación).
5. Si sustituye a un ADR anterior, actualizar el estado del antiguo a "Sustituida por ADR-NNNN".
6. Añadir una línea al índice `docs/adr/README.md`: `| NNNN | Título | tipología | estado | fecha |`.
7. Mencionar al usuario en una línea qué ADR se ha creado y dónde.
