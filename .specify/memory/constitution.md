<!--
Sync Impact Report
Version change: 1.0.0 -> 1.0.1
Modified principles: none
Updated constraints: normative wiki path aligned to the existing MyMiki checkout
Added sections: none
Removed sections: none
Follow-up TODOs: Confirm the original ratification date.
-->
# MyMiki Constitution

## Core Principles

### I. WikiText como único lenguaje de autoría
Todo contenido funcional de la MVP MUST implementarse mediante tiddlers escritos en
WikiText. Está prohibido añadir JavaScript, CSS o HTML a los tiddlers de la MVP. Las
funciones se construirán con enlaces, transclusiones, filtros, campos y macros existentes
en TiddlyWiki. Así se demuestra el modelo nativo y mantenible del producto.

### II. Castellano en toda la experiencia
Los títulos, textos visibles, nombres explicativos, comentarios y documentación MUST estar
en castellano. Se conservan sin traducir los nombres técnicos que TiddlyWiki exige para
campos, operadores, etiquetas del sistema o sintaxis. La interfaz resultante debe ser
comprensible para una persona hispanohablante.

### III. Tiddlers autocontenidos y componibles
Cada tiddler MUST tener un propósito claro y reutilizable; las vistas compuestas MUST
reutilizar contenido mediante transclusión y navegación interna en vez de duplicarlo. Los
campos y etiquetas MUST expresar metadatos útiles para filtrar o relacionar información.
Esto mantiene la MVP pequeña y permite ampliarla sin rehacer su contenido.

### IV. Ejemplo funcional de extremo a extremo
La MVP MUST mostrar un recorrido utilizable que combine contenido, navegación,
organización y una vista dinámica construida con capacidades nativas de TiddlyWiki. Cada
interacción descrita debe corresponder a una función operativa en el wiki; no se aceptan
maquetas que simulen funcionalidad.

### V. Alcance y validación explícitos
Los tiddlers creados o modificados para esta MVP MUST residir exclusivamente en
`/home/dev/projects/70_Fast_TW_SDD/MyMiki/tiddlers`. La validación MUST realizarse en una instancia real de TiddlyWiki y
comprobar la carga de los tiddlers, sus enlaces, transclusiones, filtros y flujo principal.
No se exigirán pruebas automatizadas para cambios compuestos solo por WikiText.

## Restricciones del producto

El proyecto objetivo es el wiki Node.js de `/home/dev/projects/70_Fast_TW_SDD/MyMiki`. La implementación de esta MVP se limita
a tiddlers `.tid` o al formato de tiddler que ya use esa carpeta. No se modificarán archivos
de configuración, paquetes, plugins ni temas para añadir funcionalidad. Se permite usar
funciones provistas por el TiddlyWiki instalado; cualquier dependencia de plugins externos
debe identificarse y estar ya disponible en la configuración del wiki.

## Flujo de desarrollo

Antes de implementar, la especificación MUST describir el recorrido funcional y los
criterios observables de aceptación. El plan y las tareas MUST respetar el alcance de
archivos y las restricciones de autoría. La revisión MUST verificar que cada cambio esté en
castellano, sea WikiText y no introduzca JavaScript, CSS ni HTML. La comprobación final se
hará cargando o iniciando `/home/dev/projects/70_Fast_TW_SDD/MyMiki` y siguiendo el recorrido principal.

## Governance

Esta constitución prevalece sobre convenciones de implementación incompatibles. Para
enmendarla, se debe actualizar este documento, explicar el motivo y revisar que plantillas,
especificaciones, planes y tareas sigan alineados. Los cambios que añadan o redefinan
principios incrementan la versión MINOR; cambios incompatibles con principios existentes
incrementan MAJOR; aclaraciones sin cambio normativo incrementan PATCH. En cada revisión de
una feature se comprobará el cumplimiento de sus principios y se documentarán las
excepciones justificadas en sus artefactos de diseño.

**Version**: 1.0.1 | **Ratified**: TODO(RATIFICATION_DATE): fecha original desconocida | **Last Amended**: 2026-09-27
