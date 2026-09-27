# Modelo de datos: POC de API para tareas

La unidad de almacenamiento e identidad es el tiddler. Cada entidad funcional usa un título único como identificador; la API requiere codificar el título en la ruta. Las propiedades estándar se transmiten en el nivel raíz del objeto TiddlyWeb JSON y los campos personalizados bajo `fields`.

## Entidades

### Tarea

Representa una unidad de trabajo visible en la lista de tareas.

| Campo | Requisito | Significado y validación |
|---|---|---|
| `title` | Obligatorio | Identificador único del tiddler; no vacío. |
| `tags` | Obligatorio | Incluye `Tarea`; permite seleccionar tareas desde la colección. |
| `text` | Opcional | Descripción en WikiText. |
| `fields.estado` | Obligatorio en registros nuevos | `pendiente` o `completada`; registros antiguos sin estado se muestran como pendientes hasta que se corrijan. |
| `fields.checklist` | Opcional | Título de una checklist relacionada. Debe apuntar a una checklist existente cuando se informa. |

### Checklist

Agrupa elementos de verificación asociados opcionalmente a una tarea.

| Campo | Requisito | Significado y validación |
|---|---|---|
| `title` | Obligatorio | Identificador único del tiddler; no vacío. |
| `tags` | Obligatorio | Incluye `Checklist`. |
| `text` | Opcional | Descripción breve de la lista. |
| `fields.tarea` | Opcional | Título de tarea relacionada; debe apuntar a una tarea existente cuando se informa. |

### Elemento de checklist

Paso individual que puede completarse o reabrirse independientemente.

| Campo | Requisito | Significado y validación |
|---|---|---|
| `title` | Obligatorio | Identificador único del tiddler; no vacío. |
| `tags` | Obligatorio | Incluye `Elemento de checklist`. |
| `text` | Obligatorio | Texto del paso. |
| `fields.checklist` | Obligatorio | Título de la checklist que contiene el elemento. |
| `fields.estado` | Obligatorio en registros nuevos | `pendiente` o `completada`; valor por defecto `pendiente`. |

## Relaciones y ciclo de vida

- Una tarea puede relacionarse con cero o una checklist mediante `fields.checklist`.
- Una checklist puede agrupar cero o más elementos, seleccionados por `fields.checklist`.
- Una checklist puede relacionarse con cero o una tarea mediante `fields.tarea`.
- Título es la clave única del tiddler. Crear un tiddler con título ya existente equivale a actualizarlo vía PUT; la interfaz debe pedir confirmación antes de sobrescribir.
- La transición permitida para tareas y elementos es `pendiente` ↔ `completada`.
- El borrado de tarea o elemento elimina ese tiddler; la POC no borrará automáticamente otros tiddlers relacionados. Si quedan referencias, se comunicarán como vínculos sin destino hasta que el usuario las edite.

## Formato de API

Ejemplo conceptual de campos personalizados para una tarea:

```json
{
  "title": "Preparar demo",
  "tags": "Tarea",
  "text": "Revisar el flujo principal",
  "fields": {
    "estado": "pendiente",
    "checklist": "Preparación local"
  }
}
```

Las listas de etiquetas con espacios siguen la convención TiddlyWeb de listas con doble corchete (por ejemplo, `[[Elemento de checklist]]`). En la colección, la API omite por defecto `text`; el detalle individual se carga al abrir una tarea o checklist.
