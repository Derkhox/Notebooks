# Notebooks

Une varios notebooks exportados en HTML, o varios PDF, en un solo documento en el orden que elijas, y lo deja listo para guardar como PDF.

Pensado para juntar notebooks de Jupyter exportados con `nbconvert`, aunque funciona con cualquier HTML.

## Usarlo

Abre la página, arrastra tus archivos, ordénalos y descarga.

- **HTML**: se unen en un solo archivo con el estilo original intacto, una hoja nueva por documento y portada opcional. El botón abre el diálogo de impresión para guardarlo como PDF.
- **PDF**: se unen tal cual, copiando las páginas sin recomprimir, y se descargan como un PDF único.

## Todo ocurre en tu navegador

No hay servidor ni base de datos. Los archivos se leen con `FileReader`, se procesan en memoria y se descargan desde la misma pestaña. Nunca se envían a ningún sitio.

## Qué resuelve

Al imprimir un notebook exportado suelen aparecer tres problemas, y los tres están cubiertos:

- El CSS de JupyterLab pesa unos 276 KB y se repite en cada archivo. Se emite una sola vez, así que tres notebooks pasan de 920 KB a unos 640 KB.
- Las líneas de código largas se cortan en el margen derecho de la hoja. Se ajustan con `pre-wrap`.
- Las celdas y sus salidas se parten entre páginas. Se evita con `break-inside: avoid`.

Además descarta las celdas de código vacías y conserva MathJax en el archivo descargado, para que las fórmulas se rendericen al abrirlo.

## Portada

Puedes generar una portada con institución, carrera, materia, título, alumno, docente, grupo, fecha y logo. El logo se incrusta como data URI, así que el resultado sigue siendo un archivo único.

Si tu portada ya existe como PDF: guarda el cuerpo del trabajo en PDF y luego suelta los dos archivos juntos para unirlos.

## Seguridad

- Los archivos se analizan con `DOMParser`, que construye un árbol inerte: no ejecuta scripts, no carga imágenes ni dispara manejadores de eventos.
- La vista previa vive en un `iframe` con `sandbox` sin `allow-scripts`, de modo que el contenido no puede ejecutar código.
- Del HTML de origen solo se conservan recursos externos servidos por HTTPS desde una lista corta de hosts conocidos. El resto se descarta y se avisa.
- `pdf-lib` se carga desde cdnjs con `integrity`, así que el navegador rechaza el script si no coincide byte a byte con la versión esperada.
- La página declara una `Content-Security-Policy` restrictiva, con `connect-src 'none'`: no puede abrir ninguna conexión de red.

El archivo HTML que descargas conserva el contenido original de tus documentos. Si unes HTML de una fuente en la que no confías, ese contenido se ejecutará al abrir el archivo resultante en tu navegador, igual que si abrieras el original.

## Desarrollo

Todo vive en `index.html`: sin dependencias que instalar, sin compilación. Ábrelo directamente o sírvelo con cualquier servidor estático.

La única dependencia externa es [pdf-lib](https://pdf-lib.js.org/) 1.17.1, cargada por CDN y usada solo para unir PDF.

## Licencia

MIT
