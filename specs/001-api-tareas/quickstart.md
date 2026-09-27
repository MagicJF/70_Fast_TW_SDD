# Guía de validación manual

Esta guía comprueba la POC en una instancia real. Ejecuta la interfaz en navegador visible; no uses modo headless. No requiere ni añade pruebas automatizadas.

## Requisitos previos

- mise y la versión de TiddlyWiki fijada por `/home/dev/projects/70_Fast_TW_SDD/mise.toml` están disponibles.
- Los tiddlers de la POC están instalados en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers`.
- No hay otro servidor ocupando el puerto 8090.

## Iniciar la wiki

Desde `/home/dev/projects/70_Fast_TW_SDD`:

```bash
mise exec -- tiddlywiki /home/dev/projects/70_Fast_TW_SDD/MyMiki --listen port=8090
```

Abre `http://127.0.0.1:8090` en un navegador con interfaz visible. La página de inicio de la POC debe estar en castellano.

## Recorrido de lectura

1. Abre el panel de estado del servidor. Debe mostrar disponibilidad, si el acceso es de solo lectura y la versión que informa `/status`.
2. Carga la colección. Deben aparecer solo los tiddlers etiquetados como tareas, checklists o elementos de checklist; los tiddlers del sistema no deben mostrarse.
3. Abre una tarea. Comprueba título, descripción, estado y vínculo opcional a una checklist.
4. Abre una checklist. Comprueba que cada elemento muestra su texto y estado.
5. Usa la vista renderizada de una tarea y compárala con el detalle estructurado.

## Recorrido de escritura

1. Crea una tarea con título nuevo, descripción, estado pendiente y una checklist existente.
2. Confirma que la respuesta de guardado se acepta y recarga el listado desde el servidor; el nuevo registro debe aparecer.
3. Edita la descripción y marca la tarea como completada; recarga y confirma que los cambios persisten.
4. Crea y modifica un elemento de checklist y comprueba su estado.
5. Reabre la tarea y confirma que aparece pendiente.
6. Elimina los datos de demostración creados para esta sesión y verifica mediante nuevas consultas que no aparecen.

## Errores y permisos

- Consulta un título inexistente: debe indicarse `404`/no encontrado sin mostrar datos previos como actuales.
- Prueba un tiddler de demostración cuyo título contenga espacios o acentos: las rutas deben seguir identificando el mismo título.
- En una instancia de solo lectura, verifica que se informa del permiso y que la vista no anuncia cambios.
- Interrumpe temporalmente la instancia o consulta con ella apagada: debe aparecer un error de conexión y no un mensaje de éxito.
- En todas las operaciones de escritura, confirma que una respuesta negativa o sin confirmación no actualiza optimistamente el estado visible.

## Resultado esperado

El recorrido de lectura se completa en menos de 3 minutos; la creación, edición, transición de estado y eliminación se verifican mediante lecturas posteriores. La guía cubre las comprobaciones manuales de los criterios SC-001 a SC-005 de `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/spec.md`.
