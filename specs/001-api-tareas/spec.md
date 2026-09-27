# Especificación de la feature: POC de API para tareas

**Directorio de feature**: `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas`

**Creada**: 2026-09-27

**Estado**: Borrador

**Entrada**: Descripción del usuario: «Quiero una POC MVP que explore las posibilidades de la API de TiddlyWiki, por ejemplo para una gestión de tareas y checklists. Consultar la documentación de WebServer API».

## Aclaraciones

### Sesión 2026-09-27

- Q: ¿Quieres que la POC compruebe directamente desde WikiText las operaciones de lectura y escritura del WebServer API para gestionar tareas y checklists? → A: Sí; se permite todo WikiText y la POC debe comprobar esas operaciones directamente.

## Escenarios de usuario y pruebas

### Historia 1: Consultar y organizar tareas (Prioridad: P1)

Como usuario de la wiki, quiero ver mis tareas y listas de comprobación, abrir sus detalles y distinguir su estado, para saber qué queda pendiente.

**Por qué esta prioridad**: Es la base demostrable de gestión y permite validar la lectura del contenido existente.

**Prueba independiente**: Con tareas de ejemplo cargadas, la persona consulta la lista general y una checklist, abre un elemento y encuentra título, descripción, estado y pertenencia a la lista.

**Escenarios de aceptación**:

1. **Dado** que hay tareas pendientes y completadas, **cuando** consulto la vista de tareas, **entonces** puedo distinguir ambos estados y abrir cada tarea.
2. **Dado** que existen tareas asociadas a una checklist, **cuando** abro esa checklist, **entonces** veo sus elementos y el estado de cada uno.
3. **Dado** un título de tarea existente, **cuando** solicito su contenido individual, **entonces** recibo los campos de esa tarea; si no existe, se informa que no se encontró.

---

### Historia 2: Mantener tareas y checklists (Prioridad: P1)

Como usuario, quiero crear, editar, completar y eliminar tareas, y modificar sus listas de comprobación, para mantener actualizado mi trabajo desde una interacción con el servidor.

**Por qué esta prioridad**: La escritura distingue una POC operativa de una demo de solo lectura.

**Prueba independiente**: Crear una tarea con checklist, volver a consultarla y comprobar los cambios de estado y contenido; eliminarla y verificar que ya no aparece.

**Escenarios de aceptación**:

1. **Dado** que tengo permiso de escritura, **cuando** creo o actualizo una tarea con sus campos y checklist, **entonces** el cambio queda disponible al volver a consultar la tarea.
2. **Dado** que una tarea está pendiente, **cuando** la marco completada o reabierta, **entonces** la vista refleja el nuevo estado.
3. **Dado** que una tarea existe, **cuando** la elimino, **entonces** deja de aparecer en listados y su lectura individual indica que no existe.
4. **Dado** que no tengo permiso de escritura, **cuando** intento modificar una tarea, **entonces** se informa del rechazo y los datos no se presentan como modificados.

---

### Historia 3: Conocer disponibilidad y representación del servicio (Prioridad: P2)

Como usuario o persona que evalúa la POC, quiero comprobar si el servidor admite acceso y cómo representa el contenido solicitado, para entender los límites de la integración.

**Por qué esta prioridad**: Hace visibles las capacidades de lectura del servicio y aporta contexto a las operaciones de escritura.

**Prueba independiente**: Consultar el estado del servicio, recuperar una tarea como datos y como contenido renderizado, y observar una respuesta de error para título inexistente o servicio no disponible.

**Escenarios de aceptación**:

1. **Dado** que el servidor está accesible, **cuando** consulto su estado, **entonces** veo la identidad o condición de acceso, si la instancia es de solo lectura y la versión comunicada.
2. **Dado** que una tarea existe, **cuando** solicito su representación de lectura, **entonces** recibo el contenido renderizado correspondiente.
3. **Dado** que el servicio no está accesible o rechaza la operación, **cuando** intento consultar o modificar contenido, **entonces** se muestra un error comprensible sin afirmar que la operación tuvo éxito.

### Casos límite

- La lista de tareas está vacía o no hay checklists.
- El título contiene espacios, caracteres acentuados o caracteres que deben interpretarse como parte del título.
- Se intenta crear un título ya existente o actualizar uno que fue borrado.
- El servidor informa de solo lectura, falta de autenticación o rechazo de escritura.
- La respuesta contiene tareas ajenas al conjunto de la POC o tiddlers internos del sistema.
- Una tarea carece de estado, descripción o vínculo a checklist.
- La conexión se interrumpe antes de confirmar una operación de escritura.

## Requisitos

### Requisitos funcionales

- **FR-001**: La POC MUST presentar tareas y checklists en castellano, con título, estado y relaciones entre elementos identificables.
- **FR-002**: La persona MUST poder consultar el estado del servicio y distinguir disponibilidad, acceso de solo lectura y versión cuando el servicio los comunica.
- **FR-003**: La persona MUST poder listar tareas, checklists y elementos de checklist de la POC, identificados respectivamente por las etiquetas `Tarea`, `Checklist` y `Elemento de checklist`, sin incluir por defecto contenido interno del sistema ni otros tiddlers.
- **FR-004**: La persona MUST poder consultar los campos de una tarea individual y recibir una indicación clara cuando el título no existe.
- **FR-005**: La persona MUST poder crear y actualizar una tarea con título, descripción, estado y pertenencia opcional a una checklist.
- **FR-006**: La persona MUST poder cambiar una tarea entre pendiente y completada, y ver el cambio reflejado en la lista y el detalle.
- **FR-007**: La persona MUST poder crear, consultar, actualizar y eliminar checklists y elementos de checklist, conservando una relación comprensible con su tarea o lista. Consultar incluye ver los campos de un elemento individual y los elementos agrupados en su checklist.
- **FR-008**: La persona MUST poder eliminar una tarea y confirmar su ausencia en la consulta posterior. Eliminar una tarea o checklist afecta solo al tiddler seleccionado; los elementos relacionados se conservan y muestran la relación como no disponible si el referente ya no existe.
- **FR-009**: La POC MUST presentar fallos de disponibilidad, autenticación, autorización y validación como errores comprensibles; MUST NOT informar éxito si el servidor no lo confirmó.
- **FR-010**: La POC MUST permitir comparar la vista de datos con la representación renderizada de una tarea.
- **FR-011**: La POC MUST identificar las operaciones cubiertas y sus límites, y ejercitar directamente desde WikiText `GET /status`, `GET /recipes/default/tiddlers.json`, `GET /recipes/default/tiddlers/{title}`, `PUT /recipes/default/tiddlers/{title}`, `DELETE /bags/default/tiddlers/{title}` y `GET /{title}` para renderizado. El alcance se limita al recorrido de tareas y checklists; no requiere explorar otros endpoints del WebServer API.
- **FR-012**: El recorrido MUST poder completarse en una instancia local de TiddlyWiki y usar las capacidades disponibles de WikiText y del WebServer API; no MUST añadir JavaScript, CSS, HTML, paquetes, plugins ni cambios de configuración.

### Entidades principales

- **Tarea**: Unidad de trabajo identificable por título, con descripción opcional, estado y referencia opcional a una checklist.
- **Checklist**: Conjunto nombrado de elementos de verificación, cada uno con contenido y estado completado o pendiente.
- **Elemento de checklist**: Paso individual que puede marcarse pendiente o completado y asociarse a una tarea o lista.
- **Instancia de servicio**: Wiki que expone las tareas y comunica información de disponibilidad y permisos.

## Criterios de éxito

### Resultados medibles

- **SC-001**: En una demostración local, una persona completa en menos de 3 minutos el recorrido de listar tareas, abrir una checklist y consultar una tarea.
- **SC-002**: En 10 ejecuciones completas del recorrido de crear, editar, completar y eliminar una tarea, cada operación produce un resultado visible y coherente en una consulta posterior; se registra cada ejecución como correcta solo si la consulta confirma el resultado esperado.
- **SC-003**: En 5 escenarios preparados —título ausente, servicio no disponible, solo lectura, falta de autenticación y fallo de escritura— la persona distingue que la operación no se completó. La guía manual describe cómo preparar cada escenario y qué señal observable confirma el fallo.
- **SC-004**: La POC demuestra al menos una operación de lectura de colección, lectura individual, escritura, actualización, eliminación y representación renderizada de una tarea.
- **SC-005**: Una persona puede explicar qué operaciones del servicio quedaron cubiertas y qué partes de la API no se exploraron después de revisar la POC.

## Supuestos

- El usuario principal es una persona que gestiona sus propias tareas en una wiki local o de confianza; la POC no implica colaboración multiusuario.
- “Explorar todas las posibilidades” se interpreta como probar de extremo a extremo las operaciones relevantes del WebServer API para tareas y checklists desde WikiText; no requiere cubrir endpoints ajenos a ese caso de uso.
- El alcance de esta feature se mantiene dentro de las restricciones de la constitución: WikiText en tiddlers de `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers`; se permiten todas las capacidades disponibles en WikiText, incluidas llamadas directas al WebServer API, sin incorporar JavaScript, CSS, HTML, paquetes, plugins o cambios de configuración.
- Las rutas incluidas en FR-011 delimitan las capacidades del WebServer API que la POC pretende representar, sin prescribir su implementación.
- La wiki destino de la constitución y de esta feature es `/home/dev/projects/70_Fast_TW_SDD/MyMiki`, la wiki Node.js existente en el checkout.
- La autorización y persistencia dependen de la configuración y permisos de la instancia local; no se presupone exposición pública ni se cambia la configuración del servidor como parte de esta feature.
