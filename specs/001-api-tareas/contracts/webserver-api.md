# Contrato externo: WebServer API de TiddlyWiki

Base local de la POC: `http://127.0.0.1:8090`. Los títulos en rutas deben codificarse como componentes URI. La respuesta de cada llamada se procesa desde el manejador de finalización de `tm-http-request`; un error de red o un estado HTTP no satisfactorio nunca se presenta como éxito.

| Caso | Solicitud | Resultado esperado |
|---|---|---|
| Estado de la instancia | `GET http://127.0.0.1:8090/status` | `200`, JSON con campos como `anonymous`, `read_only` y `tiddlywiki_version`. |
| Listado de tiddlers | `GET http://127.0.0.1:8090/recipes/default/tiddlers.json` | `200`, colección JSON de tiddlers no internos, sin `text` por defecto. No añadir filtros arbitrarios que la configuración no haya habilitado. |
| Leer un tiddler | `GET http://127.0.0.1:8090/recipes/default/tiddlers/{title-codificado}` | `200` y campos del tiddler; `404` para título ausente. |
| Crear o actualizar | `PUT http://127.0.0.1:8090/recipes/default/tiddlers/{title-codificado}` | Cabeceras `Content-Type: application/json` y `x-requested-with: TiddlyWiki`; cuerpo TiddlyWeb JSON; `204` al guardar. |
| Eliminar | `DELETE http://127.0.0.1:8090/bags/default/tiddlers/{title-codificado}` | Cabecera `x-requested-with: TiddlyWiki`; `204` al eliminar. |
| Leer renderizado | `GET http://127.0.0.1:8090/{title-codificado}` | `200` con renderizado del tiddler; `404` si no existe. |

## Reglas del cliente WikiText

- Cada botón envía una solicitud real mediante `tm-http-request` y presenta su estado pendiente, respuesta y error en castellano.
- Las solicitudes de escritura incluyen `x-requested-with: TiddlyWiki`; no se desactiva CSRF.
- Un `PUT` puede sobrescribir un título existente; la interfaz confirma la intención de crear o editar antes de enviarlo.
- El estado `read_only` del endpoint `/status` deshabilita controles de escritura. Una denegación recibida de todas formas se comunica sin cambiar optimistamente la vista.
- La colección se filtra localmente por etiquetas `Tarea`, `Checklist` y `Elemento de checklist`, porque no se cambiará la configuración de filtros del servidor.
- `GET` de colección no contiene el campo `text`; se obtiene el detalle desde la ruta individual cuando haga falta.
- No se envían credenciales en cuerpos ni se persisten en tiddlers de la feature.

## Referencias normativas

- [WebServer API](https://tiddlywiki.com/static/WebServer%2520API.html)
- [Get Server Status](https://tiddlywiki.com/static/WebServer%2520API%253A%2520Get%2520Server%2520Status.html)
- [Get All Tiddlers](https://tiddlywiki.com/static/WebServer%2520API%253A%2520Get%2520All%2520Tiddlers.html)
- [Get Tiddler](https://tiddlywiki.com/static/WebServer%2520API%253A%2520Get%2520Tiddler.html)
- [Put Tiddler](https://tiddlywiki.com/static/WebServer%2520API%253A%2520Put%2520Tiddler.html)
- [Delete Tiddler](https://tiddlywiki.com/static/WebServer%2520API%253A%2520Delete%2520Tiddler.html)
- [Get Rendered Tiddler](https://tiddlywiki.com/static/WebServer%2520API%253A%2520Get%2520Rendered%2520Tiddler.html)
- [TiddlyWeb JSON](https://tiddlywiki.com/static/TiddlyWeb%2520JSON%2520tiddler%2520format.html)
- [tm-http-request](https://tiddlywiki.com/static/WidgetMessage%253A%2520tm-http-request.html)
