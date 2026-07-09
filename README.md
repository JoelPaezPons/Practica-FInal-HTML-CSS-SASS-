# 🏍️ Catálogo de Motos - Arquitectura CSS (Atomic Design)

Estructura del Proyecto y Carpetas

Assets/images
En esta carpeta se encuentran todas las imagenes que se utilizaran en la pagina
`moto.jpg`: Imagen principal del producto.
`fuego.png`: Icono que marcara a las motos que esten rebajadas.
`Logo.png`:   Logo de la pagina que estara arriba en el header

#📂 src/01-external-reset/
En esta carpeta se incluye el archivo que reinicia los estilos por defecto. Su función principal es eliminar los estilos por defecto que tienen los navegadores y asi garantizar que se vera de la misma manera en todos.

En el reset aplican:
Selector Universal (`*`): Configura `box-sizing: border-box` en todos los elementos y dejamos sin márgenes y paddings para poder controlar todo mas adelante.

Elementos Multimedia (`img`, `picture`, `video`, `svg`): Se transforman en elementos de bloque (`display: block`) y se vuelven fluidos (`max-width: 100%`) asi evitamos que si no entran se desborden.

Enlaces (`a`): Quitamos el subrayado por defecto (`text-decoration: none`) y hereda el color de su contenedor padre (`color: inherit`).

Formularios y Botones (`button`, `input`): Elimina los bordes y fondos grises para despues que herede la tipografía global del proyecto (`font: inherit`) y añadiendo `cursor: pointer` a los botones para que sea mas intuitivo.

Listas (`ul`, `ol`, `li`): Eliminamos los puntos y la numeraciones.

Títulos (`h1` a `h6`): Les quitamos sus tamaños por defecto y la negrita (`font-size: inherit`) dejandonos a nosotros mas adelante elegir sus tamaños.
