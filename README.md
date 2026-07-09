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

#📂 src/02-tools/
En esta carpeta se almacenan las funciones personalizadas de Sass. En nuestro caso, contiene una funcion para transformar píxeles a unidades `rem`

Para lograrlo, primero activamos el módulo nativo `@use "sass:math";` y asi poder hacer operaciones sin utilizar `/` ya que esta obsoleta.
Creamos la función `conversion($pixels)` que realiza los siguientes pasos:
1. Elimina la unidad de píxeles dividiendo el parámetro entre `1px`.
2. Divide el número resultante entre `16` (es el predeterminado en los navegadores).
3. Lo multiplica por `1rem` para devolver el valor convertido automáticamente.

También tenemos otro archivo llamado `_index.scss` con el cual podremos utilizar la función que hemos creado fuera de esa carpeta. 

Para ello, he utilizado `@forward 'functions';`. Decidí hacerlo de esta manera ya que el antiguo `@import` está obsoleto y usar `@forward` es la mejor manera de conectar los módulos en Sass actualmente.

# 🎨 SECCIÓN CENTRAL: `src/03-tokens/`
---
### 🛠️ Arquitectura de los Tokens del Proyecto
Los Tokens son las unidades de diseño más pequeñas de nuestra web como son el caso de los colores, las fuentes y los breakpoints.
Esta carpeta se compone de 4 archivos:
### 1️⃣ `_breakpoints.scss`
En este archivo llamamos a la carpeta `02-tools` para utilizar nuestro conversor. Definimos los puntos de nuestras vistas (`$break-768` y `$break-1200`):
* **Vista Ordenador:** Pantallas mayores de 1200px.
* **Vista Tablet:** Pantallas menores de 1200px hasta 768px.
* **Vista Móvil:** Pantallas menores de 768px.
  
### 2️⃣ `_colores.scss`
Centralizamos la paleta de colores del proyecto utilizando variables nativas de CSS dentro de `:root`. Esto nos permite cambiar cualquier color de la web desde un solo lugar.
``css
:root {
    --color-white: #fff;
    --color-low-grey: #f5f5f5;
    --color-grey: #bfbfbf;
    --color-strong-grey: #535151;
    --color-black: #242221;
    --color-red: #e00000;
}
### 3️⃣ _fuentes.scss
Definimos la tipografía oficial, los pesos (font-weight) y la escala de tamaños de letra. Para mantener la web accesible pasamos todos los píxeles por nuestra función de conversión usando la sintaxis de interpolación de Sass (#{}):

4️⃣ _index.scss
Como en las carpetas anteriores, este archivo actúa como el punto de acceso central. Utiliza @forward para exponer todos los archivos anteriores (breakpoints, colores, fuentes) hacia el exterior, permitiendo que los que las demas carpetas puedan utilizar sus varibles.

