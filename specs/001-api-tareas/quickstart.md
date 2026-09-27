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

Para comprobar SC-002, repite el recorrido completo diez veces con títulos nuevos. Registra cada ejecución como correcta únicamente cuando la consulta posterior confirma la creación, edición, transición a completada y eliminación. Un solo resultado no confirmado hace que esa ejecución no cuente como correcta.

## Errores y permisos

- Consulta un título inexistente: debe indicarse `404`/no encontrado sin mostrar datos previos como actuales.
- Prueba un tiddler de demostración cuyo título contenga espacios o acentos: las rutas deben seguir identificando el mismo título.
- En una instancia de solo lectura, verifica que se informa del permiso y que la vista no anuncia cambios.
- Interrumpe temporalmente la instancia o consulta con ella apagada: debe aparecer un error de conexión y no un mensaje de éxito.
- En todas las operaciones de escritura, confirma que una respuesta negativa o sin confirmación no actualiza optimistamente el estado visible.

Para comprobar SC-003, prepara y observa estas cinco condiciones:

1. **Título ausente**: consulta un título que no exista; debe aparecer el resultado `404`/no encontrado.
2. **Servicio no disponible**: detén temporalmente la instancia local; debe aparecer un error de conexión.
3. **Solo lectura**: usa una instancia configurada como solo lectura; la vista debe indicarlo y una escritura no debe mostrarse como exitosa.
4. **Falta de autenticación**: usa una instancia o sesión que requiera autenticación y no proporciones credenciales; debe mostrarse el rechazo del servicio.
5. **Fallo de escritura**: intenta una escritura que el servidor rechace por validación o autorización; debe mostrarse el error sin actualizar la vista como si hubiera tenido éxito.

Anota para cada caso la condición usada y el mensaje/estado observado. No registres un escenario como cubierto si el servidor no produjo el rechazo esperado.

Las vistas de colección incluyen únicamente tiddlers con etiquetas `Tarea`, `Checklist` o `Elemento de checklist`. Las lecturas individuales por título consultan el tiddler solicitado y muestran su respuesta. Al eliminar una tarea o checklist, se elimina solo ese tiddler; sus elementos se conservan y muestran que el referente ya no está disponible.

La eliminación de una checklist y la de cada tipo de tiddler se comprueban de forma independiente; el borrado de un referente no debe eliminar automáticamente elementos relacionados.

## Resultado esperado

El recorrido de lectura se completa en menos de 3 minutos; las diez ejecuciones de escritura y los cinco escenarios de error quedan anotados con sus lecturas posteriores y señales observadas. Comprueba también la carga de tiddlers, enlaces, transclusiones, filtros y flujo principal. La guía cubre las comprobaciones manuales de los criterios SC-001 a SC-005 de `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/spec.md`.
