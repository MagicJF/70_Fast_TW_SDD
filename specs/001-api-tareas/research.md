# Investigación: POC de API para tareas

## Decisión: llamadas HTTP desde WikiText

Se usará el mensaje de núcleo `tm-http-request`, invocado mediante widgets de acción WikiText, para realizar solicitudes HTTP desde la interfaz y guardar el estado/respuesta en tiddlers de estado. Este mecanismo está disponible desde TiddlyWiki 5.3.0 y el entorno del proyecto tiene TiddlyWiki 5.4.1. La documentación define `method`, `url`, `body`, cabeceras `header-*`, y el manejador `oncompletion`, que recibe estado, texto de estado, error, datos y cabeceras.

**Razón**: satisface la decisión del usuario de llamar directamente al API sin autoría JavaScript y permite que la interfaz muestre la respuesta real.

**Alternativas consideradas**: usar solo el sincronizador integrado no demostraría directamente las operaciones solicitadas por la POC; añadir un plugin o un módulo JavaScript queda fuera de la constitución.

Referencia: [WidgetMessage: tm-http-request](https://tiddlywiki.com/static/WidgetMessage%253A%2520tm-http-request.html), [ejemplos WikiText GET/POST y errores](https://tiddlywiki.com/static/WidgetMessage%253A%2520tm-http-request%2520Examples.html), [TiddlyWiki 5.3.0: solicitudes HTTP en WikiText](https://tiddlywiki.com/static/Release%25205.3.0.html).

## Decisión: API integrada de TiddlyWiki en el mismo origen

Las solicitudes apuntarán a la instancia local `http://127.0.0.1:8090`. La colección, el estado, los tiddlers y las operaciones de escritura usarán las rutas documentadas de WebServer API. Al servir la interfaz desde la misma instancia, el recorrido de la POC usa el origen de esa wiki y no necesita añadir una configuración de proxy o un plugin.

**Razón**: el alcance es una wiki local y la especificación no pide integrar servidores externos.

**Alternativas consideradas**: un host remoto añadiría requisitos de autenticación, CORS y disponibilidad ajenos al MVP.

Referencias: [WebServer](https://tiddlywiki.com/static/WebServer.html), [índice de WebServer API](https://tiddlywiki.com/static/WebServer%2520API.html), [estado del servidor](https://tiddlywiki.com/static/WebServer%2520API%253A%2520Get%2520Server%2520Status.html).

## Decisión: modelo y formato de intercambio

Las tareas, checklists y elementos serán tiddlers ordinarios con etiquetas y campos acordados en `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/data-model.md`. En tránsito, los campos estándar son propiedades del objeto JSON y los campos personalizados van dentro de `fields`; los valores de campo, incluidas las listas, se representan como cadenas.

**Razón**: conserva el modelo nativo de TiddlyWiki, mantiene las relaciones filtrables y respeta el formato de intercambio de la API.

**Alternativas consideradas**: almacenar todos los elementos de una checklist en un bloque de texto opaco reduciría la capacidad de filtrar y actualizar cada elemento de manera independiente.

Referencia: [TiddlyWeb JSON tiddler format](https://tiddlywiki.com/static/TiddlyWeb%2520JSON%2520tiddler%2520format.html).

## Decisión: lectura de colección y filtrado

`GET /recipes/default/tiddlers.json` devuelve por defecto tiddlers no internos sin el campo `text`. La documentación advierte que la API restringe los filtros admitidos si el servidor no ha habilitado uno explícitamente. Como la feature no puede cambiar configuración, la POC solicitará la colección permitida y seleccionará localmente los tiddlers de tarea/checklist mediante sus etiquetas; cargará cada detalle por su ruta individual cuando se necesite el texto.

**Razón**: cumple la restricción de no abrir filtros externos ni alterar la configuración de seguridad del servidor.

**Alternativas consideradas**: activar filtros arbitrarios mediante tiddlers de configuración requiere una modificación prohibida y no es necesaria para un conjunto pequeño de demostración.

Referencia: [Get All Tiddlers](https://tiddlywiki.com/static/WebServer%2520API%253A%2520Get%2520All%2520Tiddlers.html), [Get Tiddler](https://tiddlywiki.com/static/WebServer%2520API%253A%2520Get%2520Tiddler.html).

## Decisión: crear, actualizar y borrar

La creación y actualización se cubrirán con `PUT /recipes/default/tiddlers/{title}` y un cuerpo en formato TiddlyWeb JSON. La eliminación usará `DELETE /bags/default/tiddlers/{title}`. Ambas operaciones requieren `x-requested-with: TiddlyWiki` salvo que el servidor desactive CSRF; la POC siempre enviará la cabecera y no modificará el ajuste CSRF.

**Razón**: sigue la interfaz documentada y evita relajar controles del servidor.

**Alternativas consideradas**: desactivar la protección CSRF o manipular ficheros directamente eludiría el contrato HTTP y excedería el alcance.

Referencias: [Put Tiddler](https://tiddlywiki.com/static/WebServer%2520API%253A%2520Put%2520Tiddler.html), [Delete Tiddler](https://tiddlywiki.com/static/WebServer%2520API%253A%2520Delete%2520Tiddler.html), [CSRF parameter](https://tiddlywiki.com/static/WebServer%2520Parameter%253A%2520csrf-disable.html).

## Decisión: autenticación y confianza

La instancia de demostración será local y de confianza. La POC expondrá errores HTTP de autenticación/autorización y el estado `read_only`; no guardará usuario ni contraseña en tiddlers ni ampliará la configuración de autenticación.

**Razón**: permite demostrar denegaciones sin crear un sistema de cuentas para una POC individual.

**Alternativas consideradas**: desplegar credenciales nuevas o exponer el servidor a Internet contradice el alcance y requeriría diseño de seguridad adicional.

Referencia: [WebServer Authorization](https://tiddlywiki.com/static/WebServer%2520Authorization.html).
