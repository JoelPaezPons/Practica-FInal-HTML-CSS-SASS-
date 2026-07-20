# 🏍️ Catálogo de Motos - Arquitectura CSS (Atomic Design)

Estructura del Proyecto y Carpetas

Assets/images
En esta carpeta se encuentran todas las imagenes que se utilizaran en la pagina
`moto.jpg`: Imagen principal del producto.
`fuego.png`: Icono que marcara a las motos que esten rebajadas.
`Logo.png`:   Logo de la pagina que estara arriba en el header
`User.png`:   Icono de usuario que aparece en el header

#📂 src/01-external-reset/
---
En esta carpeta se incluye el archivo que reinicia los estilos por defecto. Su función principal es eliminar los estilos por defecto que tienen los navegadores y asi garantizar que se vera de la misma manera en todos.

En el reset aplican:
En el reset aplican:

Selector Universal (`*`): Configura box-sizing: border-box en todos los elementos y dejamos sin márgenes y paddings para poder controlar todo mas adelante. Esto es importante porque con border-box, el padding y el border que le pongamos a un elemento se cuentan dentro de su ancho/alto declarado, esto puede hacer que mas adelante se puedan romper.

Elementos Multimedia (`img`, `picture`, `video`, `svg`): Se transforman en elementos de bloque (`display: block`) y se vuelven fluidos (`max-width: 100%`) asi evitamos que si no entran se desborden.

Enlaces (`a`): Quitamos el subrayado por defecto (`text-decoration: none`) y hereda el color de su contenedor padre (`color: inherit`).

Formularios y Botones (`button`, `input`): Elimina los bordes y fondos grises para despues que herede la tipografía global del proyecto (`font: inherit`) y añadiendo `cursor: pointer` a los botones para que sea mas intuitivo.

Listas (`ul`, `ol`, `li`): Eliminamos los puntos y la numeraciones.

Títulos (`h1` a `h6`): Les quitamos sus tamaños por defecto y la negrita (`font-size: inherit`) dejandonos a nosotros mas adelante elegir sus tamaños.

También añadimos body `{ font-family: var(--font-family-base); }`, para que toda la tipografía del proyecto (Arial) se herede automáticamente a cualquier elemento, sin tener que repetir la propiedad en cada componente por separado.

#📂 src/02-tools/
---
En esta carpeta se almacenan las funciones personalizadas de Sass. En nuestro caso, contiene una funcion para transformar píxeles a unidades rem, llamada `px-to-rem`

Para lograrlo, primero activamos el módulo nativo `@use "sass:math";` y asi poder hacer operaciones sin utilizar `/` ya que esta obsoleta.
Creamos la función `px-to-rem($pixels)` que realiza los siguientes pasos:
1. Elimina la unidad de píxeles dividiendo el parámetro entre `1px`.
2. Divide el número resultante entre `16` (es el predeterminado en los navegadores).
3. Lo multiplica por `1rem` para devolver el valor convertido automáticamente.

Le pusimos el nombre px-to-rem en vez del nombre genérico que tenía al principio (conversion), porque describe exactamente lo que hace: convierte de píxeles a rem.

Usamos esta función en todo el proyecto en vez de escribir rem directamente, por dos motivos:

Trabajar en píxeles es más cómodo al medir sobre el diseño de Figma, que siempre da las medidas en px.
El resultado final sigue siendo rem, que es más accesible: si el usuario cambia el tamaño de letra base de su navegador, toda la web escala proporcionalmente, en vez de quedarse fija con un tamaño en píxeles.

También tenemos otro archivo llamado `_index.scss` con el cual podremos utilizar la función que hemos creado fuera de esa carpeta. 

Para ello, he utilizado `@forward 'functions';`. Decidí hacerlo de esta manera ya que el antiguo `@import` está obsoleto y usar `@forward` es la mejor manera de conectar los módulos en Sass actualmente.

## 📂 src/03-tokens/
---
Los Tokens son las unidades de diseño más pequeñas de nuestra web como son el caso de los colores, las fuentes y los breakpoints.
Esta carpeta se compone de 4 archivos:
### 1️⃣ `_breakpoints.scss`
En este archivo llamamos a la carpeta `02-tools` para utilizar nuestro conversor. Definimos los puntos de nuestras vistas (`$break-768` y `$break-1200`):
* **Vista Ordenador:** Pantallas mayores de 1200px.
* **Vista Tablet:** Pantallas menores de 1200px hasta 768px.
* **Vista Móvil:** Pantallas menores de 768px.

### 2️⃣ `_colors.scss`
Centralizamos la paleta de colores del proyecto utilizando variables nativas de CSS dentro de `:root`. Esto nos permite cambiar cualquier color de la web desde un solo lugar.
@mixin colors {
    --color-white: #fff;
    --color-low-grey: #f5f5f5;
    --color-grey: #bfbfbf;
    --color-strong-grey: #535151;
    --color-black: #242221;
    --color-red: #e00000;
    --color-blue: #1E24BD;
}
### 3️⃣ _fonts.scss
Definimos la tipografía oficial, los pesos (font-weight) y la escala de tamaños de letra. Para mantener la web accesible pasamos todos los píxeles por nuestra función px-to-rem usando la sintaxis de interpolación de Sass (#{})

Los tamaños se llaman --font-size-38, --font-size-28, --font-size-24, --font-size-16, --font-size-14 y --font-size-12 — el número es el valor real en píxeles de cada uno.
Al principio los llamábamos con nombres tipo --font-size-xl, --font-size-lg, --font-size-sm... pero tuve que cambiar los nombres porque no eran intuitivos: para saber cuánto medía --font-size-lg había que ir a abrir el archivo y mirarlo. Con el número en el propio nombre, cualquiera que lea el código sabe el tamaño exacto sin necesidad de comprobar nada.


### 4️⃣ _icons.scss
Definimos aquí una escala de tamaños pensada específicamente para iconos (--icon-size-2 hasta --icon-size-64, también nombrados por su valor real en píxeles, por el mismo motivo que los tamaños de fuente). La separamos de _fonts.scss porque, aunque técnicamente también son medidas, no son tipografía, y mezclarlas hacía más difícil encontrar cada cosa.

El logo del header (.header__logo) es la única excepción: no usa ninguna variable de _icons.scss, sino px-to-rem(70px) directamente en _header.scss. Esto es porque el logo no es un icono cuadrado como el resto (mide 70×48px en el diseño, una proporción rectangular), y es el único elemento de la web con esa medida exacta — no tenía sentido crear una variable reutilizable para un valor que solo se usa una vez.


### 5️⃣ _root.scss
Este archivo junta los tres mixins anteriores (colors, fonts, icons) dentro de un único bloque :root:

@use 'colors';
@use 'fonts';
@use 'icons';

:root {
    @include colors.colors;
    @include fonts.fonts;
    @include icons.icons;
}

Se creo porque, al principio, cada archivo de tokens tenía su propio :root { ... } por separado. Esto compilaba sin errores, pero generaba un style.css final con varios bloques :root repetidos, uno por archivo haciendo algo incesario. Convirtiendo cada archivo en un @mixin y uniéndolos aquí con @include, conseguimos que el CSS final tenga un único :root con todas las variables juntas.

### 6️⃣ _index.scss
Como en las carpetas anteriores, este archivo actúa como el punto de acceso central. Utiliza @forward para exponer los 5 archivos anteriores hacia el exterior, permitiendo que las demas carpetas puedan utilizar sus varibles con un único @use '../03-tokens/' as tokens.

## 📂 src/04-atoms/
---
Los atoms son los componentes más pequeños y genéricos del proyecto: no dependen de ningún contexto concreto, son como piezas sueltas que se reutilizan en cualquier sitio.

`_buttons.scss`
.btn define los estilos base compartidos por todos los botones (flex, tipografía, gap). A partir de ahí, cada variante añade lo suyo:

  .btn--header: El botón oscuro que se usa tanto en "Ver todas las motos" del header como en "Suscríbete" del newsletter.
  .btn-filter: Los botones de filtro del catálogo, con un estado adicional .btn-filter.is-active para marcar visualmente cuáles están seleccionados.
  .btn-configuration: El botón de acción dentro de cada card.

`_images.scss`
.images-responsive: Comportamiento genérico para cualquier imagen del proyecto (se combina en el HTML con clases más específicas, como .card-image o .header__logo, para no repetir max-width/height en cada sitio).

`_inputs.scss`
.input: estilo del campo de email del formulario de newsletter (borde, padding, tamaño de letra, y flex-grow: 1 para que ocupe todo el espacio disponible dentro de su contenedor).

`_links.scss`
.link-text: Estilo compartido por cualquier enlace de texto en azul y subrayado. Se usa tanto en "Ver detalles" de las cards como en los 6 enlaces del footer, para no duplicar la misma regla dos veces en dos sitios distintos del proyecto.

`_tags.scss`
.tag: Las etiquetas descriptivas de cada moto (Eléctrica, Automática). No tiene un font-size propio a propósito — hereda el tamaño base del body (16px), que es el tamaño que pedía el diseño para este elemento.

`_titles.scss`
.title: Peso de fuente (negrita) y line-height base, compartidos por todos los títulos del proyecto que necesitan verse en negrita. Se combina con una clase de tamaño (.title-page, .title-page-subtitle, .title-card) para dar el tamaño concreto según dónde se use. El <h2> con el nombre de cada moto dentro de la card (.title-card) usa un peso de letra normal, sin la clase .title, ya que en el diseño ese texto en concreto no lleva negrita.


## 📂 src/05-molecules/
---
Las molecules combinan varios atoms para formar un componente con sentido propio, pero que se sigue repitiendo/reutilizando.

`_card.scss`
La tarjeta de producto completa: imagen, cuerpo en columna con flexbox, fila de tags, fila de precio + enlace "Ver detalles" (con justify-content: space-between para separarlos a los extremos), y el icono de descuento alineado con el texto mediante display: flex; align-items: center;.

El texto "(con IVA)" está envuelto en un `<span class="card-price-vat">` aparte, con un font-size distinto (14px) al del precio principal (16px). Usamos <span> porque es la única etiqueta HTML pensada exactamente para este caso: aplicar un estilo distinto a una porción de texto sin significado semántico especial y sin romper el flujo en línea de la frase a diferencia de un <div> o un <p>, que son elementos de bloque y forzarían un salto de línea.

.card no lleva padding en ningún punto. Aunque en un primer momento pensamos que hacía falta para separar el contenido del borde de la tarjeta, decidimos mantenerlo así porque, con la card completa (imagen + cuerpo de texto) todos sus elementos internos ya quedan alineados de forma simétrica gracias al propio padding de .main-content y al gap entre cards del grid.


## 📂 src/06-organisms/
---
Los organisms son los bloques estructurales grandes de la página que aparecen una única vez (a diferencia de las cards, que se repiten).

`_header.scss`
La cabecera de la web: Logo, botón "Ver todas las motos", bloque de usuario (icono + texto) y menú hamburguesa (hecho con tres <span>), En vista móvil (por debajo de 768px), el botón "Ver todas las motos" (.header__cta) y el bloque de usuario (.header__user) se ocultan con display: none, dejando visibles solo el logo y la hamburguesa.

El padding-inline del header (32px) coincide con el padding-inline de .main-content, para que el logo y el título "Todas las motos" queden alineados verticalmente en el mismo margen izquierdo de la página.

`_footer.scss`
El pie de página, con los 6 enlaces legales centrados y separados con gap, y una línea superior fina (border-top) en gris, para que sea un separador sutil respecto al resto de la página.

`_subscription.scss`
La sección de suscripción con su formulario (.form, .form__field). El campo de email crece con flex-grow: 1 para ocupar el espacio que el botón "Suscríbete" no usa, y todo el bloque va envuelto en .subscription__container, que limita el ancho a un máximo de 500px (medido directamente del diseño de Figma) para que el formulario no se estire de forma desproporcionada en pantallas muy anchas.


##📂 src/07_pages/
---
Esta carpeta contiene los estilos específicos de una página concreta de la web.

`_distribution.scss`
Incluye .page-layout y .main-content (la estructura general de cualquier página, con flex-grow: 1 para que el contenido empuje el footer hacia abajo aunque haya poco contenido), y el contenido propio del catálogo: .catalog-header, .filters y .distribution (el grid de cards).

.filters usa display: grid; grid-template-columns: repeat(3, 1fr); en vista móvil, en vez de display: flex; flex-wrap: wrap;. Se hizo así porque necesitábamos un resultado exacto (los 3 primeros filtros en la fila de arriba, y el cuarto Diesel siempre solo en la fila de abajo), y con flex-wrap el punto de corte depende del ancho real de cada botón y podía variar de forma menos controlada según el contenido. Con el grid de 3 columnas fijas, el 4º elemento pasa automáticamente a la siguiente fila sin necesitar ninguna clase adicional.

.distribution sí cambia de columnas según el breakpoint (1 en móvil, 2 en tablet, 3 en ordenador), por si el numero de cards variara en un futuro y queremos que el grid se adapte solo, sin tener que forzar ninguna posición concreta.
