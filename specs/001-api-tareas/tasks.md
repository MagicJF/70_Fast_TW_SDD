# Tareas: POC de API para tareas

**Entrada**: Documentos de diseño en `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/`.

**Prerequisitos**: `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/plan.md`, `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/spec.md`, `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/research.md`, `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/data-model.md` y `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/contracts/webserver-api.md`.

**Pruebas**: No se incluyen tareas de pruebas automatizadas. La constitución pide validar manualmente en TiddlyWiki real; la comprobación manual está en la fase final.

**Organización**: Las tareas de las historias de usuario están separadas para implementar y validar cada recorrido independientemente.

## Fase 1: Preparación

**Propósito**: El repositorio ya contiene la wiki Node.js, TiddlyWiki 5.4.1 y el plugin TiddlyWeb. No se requieren cambios de configuración, paquetes ni estructura de proyecto.

## Fase 2: Base común

**Propósito**: Añadir datos de demostración y las definiciones WikiText que las historias utilizan.

- [ ] T001 [P] Crear tiddler de tarea pendiente con `title` obligatorio y no vacío, etiqueta `Tarea`, `fields.estado` igual a `pendiente` o `completada` y `fields.checklist` opcional en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/Tarea - Preparar la demo.tid`
- [ ] T002 [P] Crear tiddler de tarea completada con `title` obligatorio y no vacío, etiqueta `Tarea` y `fields.estado` igual a `pendiente` o `completada` en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/Tarea - Revisar documentación.tid`
- [ ] T003 [P] Crear tiddler de checklist con `title` obligatorio y no vacío, etiqueta `Checklist` y `fields.tarea` opcional que apunta a una tarea existente en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/Checklist - Preparación de demo.tid`
- [ ] T004 [P] Crear dos tiddlers de elemento con `title` obligatorio y no vacío, texto obligatorio, etiqueta `Elemento de checklist`, `fields.checklist` obligatorio y `fields.estado` igual a `pendiente` o `completada` en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/Elemento - Verificar lectura.tid` y `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/Elemento - Verificar escritura.tid`
- [ ] T005 [P] Crear tiddler global de procedimientos WikiText para enviar solicitudes con `tm-http-request`, mostrar estado pendiente y capturar estado HTTP, texto de estado, datos, cabeceras y error en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - API - solicitudes HTTP.tid`

**Punto de control**: La base no introduce JavaScript, CSS, HTML, plugins ni cambios de configuración; contiene ejemplos distinguibles por etiqueta y estado.

---

## Fase 3: Historia 1 - Consultar y organizar tareas (Prioridad: P1) 🎯 MVP

**Objetivo**: Consultar tareas, checklists y detalles reales desde el servidor.

**Prueba independiente**: Con los tiddlers iniciales, cargar la colección desde el servidor, ver tareas pendientes y completadas, abrir una checklist, y recuperar una tarea por título incluyendo el caso de título inexistente.

### Implementación

- [ ] T006 [P] [US1] Crear página de inicio en castellano con navegación a tareas, checklists, estado del servidor y ayuda en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - API de tareas.tid`
- [ ] T007 [P] [US1] Crear vista de tareas que solicite `GET /recipes/default/tiddlers.json`, seleccione localmente la etiqueta `Tarea`, represente título y estado, y distinga respuesta vacía y error en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Lista de tareas.tid`
- [ ] T008 [P] [US1] Crear vista de checklists que solicite la colección y agrupe las etiquetas `Checklist` y `Elemento de checklist` por `fields.checklist`, mostrando estado vacío y estado de cada elemento en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Lista de comprobación.tid`
- [ ] T009 [US1] Crear detalle de tarea que codifique el título en la ruta, solicite `GET /recipes/default/tiddlers/{title}`, y muestre título, descripción, estado, checklist relacionada y errores HTTP en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Detalle de tarea.tid`

**Punto de control**: El listado y el detalle reflejan datos recibidos del WebServer API; no contienen respuestas ficticias.

---

## Fase 4: Historia 2 - Mantener tareas y checklists (Prioridad: P1)

**Objetivo**: Crear, actualizar, completar, reabrir y eliminar tareas y elementos de checklist desde WikiText.

**Prueba independiente**: Crear una tarea y elementos nuevos, consultarlos de nuevo desde el servidor, editar campos, cambiar estados y borrar los registros de demostración.

### Implementación

- [ ] T010 [US2] Crear formulario WikiText de tarea que valide `title` obligatorio y no vacío, `fields.estado` con valores `pendiente` o `completada`, descripción `text` opcional y `fields.checklist` opcional con referencia existente en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Editar tarea.tid`
- [ ] T011 [US2] Implementar en el formulario de tarea el envío de creación/actualización mediante `PUT /recipes/default/tiddlers/{title-codificado}`, cuerpo TiddlyWeb JSON y cabeceras `Content-Type: application/json` y `x-requested-with: TiddlyWiki`; impedir el envío cuando `/status` indica `read_only` y reflejar éxito solo ante `204` en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Editar tarea.tid`
- [ ] T012 [P] [US2] Crear formulario WikiText para crear, consultar y actualizar checklists y elementos: `title` obligatorio y no vacío; `fields.tarea` opcional con referencia existente; texto del elemento obligatorio; `fields.checklist` obligatorio con referencia existente; `fields.estado` igual a `pendiente` o `completada` en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Editar checklist.tid`
- [ ] T013 [US2] Añadir eliminación confirmada de tareas, checklists y elementos con `DELETE /bags/default/tiddlers/{title-codificado}` y `x-requested-with: TiddlyWiki`; tras `204`, volver a cargar la colección y comunicar errores sin alterar la vista en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Editar tarea.tid` y `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Editar checklist.tid`

**Punto de control**: Cada cambio persiste tras una lectura posterior; una operación rechazada no aparece como exitosa.

---

## Fase 5: Historia 3 - Conocer disponibilidad y representación del servicio (Prioridad: P2)

**Objetivo**: Presentar estado y permisos de la instancia, comparar datos con renderizado y explicar fallos del servicio.

**Prueba independiente**: Consultar `/status`, comprobar el modo de solo lectura, cargar el renderizado de una tarea y observar errores de título inexistente, autenticación, permiso y disponibilidad.

### Implementación

- [ ] T014 [P] [US3] Crear vista de estado que solicite `GET /status` y presente disponibilidad, `anonymous`, `read_only` y `tiddlywiki_version`; ocultar o deshabilitar escritura si `read_only` es verdadero en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Estado del servidor.tid`
- [ ] T015 [P] [US3] Crear vista de representación que solicite `GET /{title-codificado}` y permita comparar el renderizado con los campos de la tarea en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Representación de tarea.tid`
- [ ] T016 [US3] Completar mensajes de error en castellano para desconexión, título no encontrado, autenticación, autorización, respuesta HTTP inesperada y respuesta de escritura no confirmada en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - API - solicitudes HTTP.tid`

**Punto de control**: La persona puede distinguir error, solo lectura y éxito confirmado; el estado de la instancia no se confunde con el estado de las tareas.

---

## Fase 6: Pulido y validación transversal

**Propósito**: Documentar el alcance demostrado y recorrer manualmente las capacidades de extremo a extremo.

- [ ] T017 [P] Documentar en castellano las operaciones demostradas y los endpoints fuera del alcance de tareas/checklists en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Alcance del WebServer API.tid`
- [ ] T018 Ejecutar el recorrido manual de `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/quickstart.md` en la instancia real y navegador visible; corregir problemas solo en tiddlers bajo `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers`
- [ ] T019 Revisar todos los tiddlers de la feature contra la constitución: castellano, WikiText, sin JavaScript/CSS/HTML y guardados exclusivamente en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers`

## Dependencias y orden de ejecución

### Dependencias entre fases

- Preparación: no requiere tareas; la wiki y las dependencias ya están presentes.
- Base común: debe completarse antes de las historias, pues aporta datos iniciales y procedimientos compartidos.
- Historias 1, 2 y 3: pueden implementarse en paralelo una vez terminada la base común; se recomienda validar primero la Historia 1 como MVP.
- Pulido: requiere las historias seleccionadas para la entrega; T018 requiere los recorridos terminados.

### Dependencias por historia

- Historia 1 (P1): depende de T001–T005; independiente de US2 y US3.
- Historia 2 (P1): depende de T001–T005; independiente de US1 y US3.
- Historia 3 (P2): depende de T001–T005; independiente de US1 y US2.

### Oportunidades de paralelización

- T001–T005 escriben tiddlers separados y pueden prepararse en paralelo.
- T006–T008 escriben tiddlers separados y pueden desarrollarse en paralelo; T009 se integra con la navegación de T006.
- T010/T011 comparten un tiddler, por lo que deben ejecutarse en secuencia; T012 puede avanzar en paralelo al formulario de tarea. T013 requiere que los controles de T010–T012 existan.
- T014 y T015 usan tiddlers separados y pueden implementarse en paralelo; T016 modifica el tiddler de procedimientos HTTP y debe coordinarse con T005.
- T017 puede redactarse en paralelo con las historias. T018 y T019 son pasos finales.

## Ejemplos de ejecución en paralelo

Una vez terminada la base común, se pueden asignar vistas independientes:

```text
T007 -> /home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Lista de tareas.tid
T008 -> /home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Lista de comprobación.tid
T014 -> /home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Estado del servidor.tid
T015 -> /home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/POC - Representación de tarea.tid
```

Cada tarea paralela mantiene su propio archivo. Las tareas T010/T011/T013 no deben editarse simultáneamente porque comparten el formulario de tarea.

## Estrategia de implementación

### MVP primero

1. Completar T001–T009.
2. Ejecutar el criterio independiente de la Historia 1 y detenerse si las consultas del API no alimentan las vistas correctamente.
3. Añadir CRUD de la Historia 2 y validar cada operación con una consulta posterior.
4. Añadir estado y renderizado de la Historia 3.
5. Completar T017–T019.

### Entrega incremental

Cada historia usa el contrato del API y los procedimientos comunes; las vistas y formularios permanecen en tiddlers separados. El incremento inicial viable es US1: listado real, checklist y detalle. US2 añade persistencia de cambios; US3 añade diagnóstico del servidor y renderizado.
