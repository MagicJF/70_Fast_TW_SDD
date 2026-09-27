# Plan de implementación: POC de API para tareas

**Rama**: `001-api-tareas` | **Fecha**: 2026-09-27 | **Especificación**: [spec.md](/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/spec.md)

**Entrada**: Especificación de feature en `/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/spec.md`.

## Resumen

Construir en WikiText una POC en castellano para gestionar tareas y checklists que ejercite directamente la WebServer API de la wiki local. El recorrido cubrirá estado del servidor, lectura de colección y tiddlers, renderizado, alta/actualización mediante `PUT` y eliminación mediante `DELETE`; presentará estados HTTP y errores sin simular éxitos. Las solicitudes se harán con el mecanismo `tm-http-request` disponible en TiddlyWiki 5.4.1. Todo el contenido funcional nuevo residirá en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers`.

## Contexto técnico

**Lenguaje/versión**: WikiText de TiddlyWiki 5.4.1; el checkout fija Node.js 24 mediante mise.

**Dependencias principales**: TiddlyWiki 5.4.1, plugin `tiddlywiki/tiddlyweb` ya declarado en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlywiki.info` y el mensaje `tm-http-request` del núcleo (disponible desde TiddlyWiki 5.3.0).

**Almacenamiento**: Tiddlers de la wiki Node.js local, persistidos por el adaptador de archivos ya configurado.

**Validación**: Recorrido manual en la instancia real de TiddlyWiki, con navegador visible; se comprueban contenido, navegación, filtros, lectura/escritura HTTP y errores. No se añadirán pruebas automatizadas para la funcionalidad escrita solo en WikiText, de acuerdo con la constitución.

**Plataforma objetivo**: `/home/dev/projects/70_Fast_TW_SDD/MyMiki` servida localmente en Node.js, inicialmente en `http://127.0.0.1:8090`.

**Tipo de proyecto**: Wiki TiddlyWiki de un solo proyecto; no se añaden servicios, clientes ni paquetes separados.

**Objetivos de rendimiento**: Completar el recorrido primario de lectura en menos de 3 minutos. La POC no establece objetivos de concurrencia ni latencia de servicio.

**Restricciones**: Autoría funcional solo con tiddlers WikiText en `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers`; sin JavaScript, CSS, HTML, paquetes, plugins nuevos ni cambios de configuración. Las operaciones de escritura HTTP deben incluir `x-requested-with: TiddlyWiki`. No se deben guardar credenciales en tiddlers. La instancia se considera local o de confianza.

**Escala/alcance**: Uso individual en una wiki local y un conjunto pequeño de tareas de demostración; sin colaboración simultánea ni uso público.

## Revisión de constitución

| Principio o restricción | Estado | Aplicación al plan |
|---|---|---|
| WikiText como único lenguaje de autoría | Conforme | `tm-http-request` se invoca desde acciones WikiText existentes; no se implementan módulos JavaScript. |
| Castellano en la experiencia | Conforme | Interfaz, títulos explicativos, errores y documentación en castellano; se conservan nombres técnicos obligatorios. |
| Tiddlers autocontenidos y componibles | Conforme | Vistas, procedimientos y transclusiones se distribuyen en tiddlers con responsabilidades claras. |
| Recorrido funcional de extremo a extremo | Conforme | Las solicitudes reales alimentan el estado visible; no hay respuestas simuladas. |
| Ruta y validación explícitas | Conforme | El destino se alinea con la wiki existente en `/home/dev/projects/70_Fast_TW_SDD/MyMiki`; validación en servidor real y navegador visible. |
| Sin cambios de configuración o dependencias | Conforme | Se usan el plugin y las funciones ya disponibles; no se cambia `tiddlywiki.info`. |

**Resultado del gate**: Aprobado antes y después del diseño. La diferencia entre el nombre anterior `MyWiki` de la constitución y la wiki existente `MyMiki` queda resuelta alineando la ruta normativa con `/home/dev/projects/70_Fast_TW_SDD/MyMiki`.

## Estructura del proyecto

### Documentación de la feature

```text
/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/plan.md
/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/research.md
/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/data-model.md
/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/quickstart.md
/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/contracts/webserver-api.md
/home/dev/projects/70_Fast_TW_SDD/specs/001-api-tareas/tasks.md  (fase posterior: $speckit-tasks)
```

### Contenido funcional

```text
/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers/
```

**Decisión de estructura**: Una sola wiki; todos los tiddlers funcionales de la POC estarán directamente en la carpeta existente `/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers`. Los artefactos de diseño permanecerán en la carpeta de feature. No se crean directorios `src/` ni `tests/`.
