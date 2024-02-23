# Documentación Apache2

## Instalación
Instalación de apache2:
Instalamos el servicio con los privilegios de administrador.
```bash
sudo apt install apache2
```

## Creación de la página
Para crear una página hay que dirigirse a la dirección `/var/www/`, donde veremos un fichero llamado `index.html`.
Para crear la primera página, creamos un directorio dentro de la ubicación anterior con el nombre del dominio (por organización) que queramos, este tiene que acabar en un prefijo `.es, .org, .com`...
Posteriormente accedemos al interior del directorio creado y ahi dentro creamos un fichero llamado `index.html`.
```bash
#Nos dirigimos a la ubicación mencionada
cd /var/www/
# - Ubicación actual: /var/wwww

#Creamos el directorio con el prefijo
mkdir mipagina.es
# - Ubicación actual: /var/www

#Accedemos al directorio
cd mipagina.es
# - Ubicación actual: /var/www/mipagina.es

#Creamos el fichero dentro del directorio creado
touch index.html
# - Ubicación actual: /var/www/mipagina.es
```

## Edición de la página
Para dar estructura a la página, hay que editar el fichero `index.html` creado anteriormente, para ello es necesario tener los conocimientos básicos de HTML5.
Si se quiere agregar diseño y funcionalidad, es requerido usar ficheros `.css` y `.js`.
```bash
#Editamos el fichero index.html
sudo nano index.html
# - Ubicación actual: /var/www/mipagina.es/index.html
```

Dentro del fichero `index.html` agregamos la estructura básica de HTML5:
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MiPagina</title>
    <!--Elimina la siguiente linea si no quieres aplicar estilos-->
    <link href="styles/style.css" rel="stylesheet" type="text/css" />
</head>
<body>
    <!--Agrega el contenido que veas necesario-->
    <p>Este es el contenido de tu pagina</p>
    <!--Elimina la siguiente linea si no quieres aplicar estilos-->
    <script src="scripts/script.js"></script>
</body>
</html>
```

En el código anterior vemos comentarios con la opcion de dejar lineas que permiten agregar codigo CSS y JavaScript, esto es completamente opcional, se recomienda hacer eso lo últmo.
Para poner un ejemplo de como sería la estructura con la implementación de estilos y scripts tnemos que tener en cuenta que estos ficheros se deberian agregar en subdirectorios dentro de la ruta de la pagina principal.
Esto permitira un acceso rapido, asi como una buena organizacion, en el siguiente ejemplo se presenta la estructura ideal para este proceso.

## Estructura de directorio
```
var
└── www
    └── mipagina.es
        ├── index.html
        ├── styles
        │   └── style.css
        └── scripts
            └── script.js
```

Para crear esta misma estructura hay que estar dentro de la ruta de la pagina principal `mipagina.es` y crear dos directorios, el primero seria `styles`, donde almacenaremos los ficheros de estilos `.css` y el segundo `scripts`, donde almacenaremos ficheros `.js`.

Para llevar a cabo este proceso opcional, hay que hace lo siguiente:
```bash
#Nos posicionamos dentro de la ruta de la pagina
cd /var/www/mipagina.es
# - Ubicación actual: /var/www/mipagina.es

#Creamos dos directorios
mkdir styles
mkdir scripts
# - Ubicación actual: /var/www/mipagina.es
```
## Estilo y Funcionamiento de la pagina con CSS y JS

Tras la creacion de los directorios, deberemos llenarlos con el codigo necesario, este estara almacenado dentro de ficheros, los cuales son llamados por la pagina principal `index.html`.
Para crear estos ficheros es necesario acceder a cada directorio y crear un fichero con su correspondiente contenido:
```bash
#Nos posicionamos dentro de la ruta JavaScript (scripts)
cd /var/www/mipagina.es/scripts
# - Ubicación actual: /var/www/mipagina.es/scripts

#Creamos el primer fichero de scripts con touch, esto nos abrira una ventana para introducir texto
sudo touch script.js
# - Ubicación actual: /var/www/mipagina.es/scripts/script.js
```

Tras crear el fichero dentro de la ruta especificada, nos abrira una ventana donde ingresar texto plano, en esta ventana ingresaremos codigo JavaScript, el cual estara vinculado a la pagina principal (debido al nombre y ruta usados, estos mismos estan referenciados en la pagina web, por lo que si se quiere, se puede cambiar la ruta de la pagina, no obstante, para su correcto funcionamiento es recomendable usar una ruta clara y fija.

El codigo del fichero JavaScript puede ser el que se desee, en este caso se usara un codigo simple como muestra de correcto funcionamiento:
```js
//Alerta que muestra el correcto funcionamiento del codigo
alert("El Codigo JavaScript Funciona correctamente");
//Log en consola indicando el correcto funcionamiento del mismo
console.log("Correcto funcionamiento");
```

Para poder aplicar estilos en la pagina, es necesario usar el mismo sistema usado durante la creacion de scripts de `JavaScript`, para ello iremos a la ruta de los estilos creada anteriormente y crearemos un fichero donde almacenaremos el codigo de los estilos de la apgina principal, este fichero debera contener el nombre referenciado en la pagina web, el cual es `style.css`, para que pueda funcionar, asi como el codigo `.js` debera estar en la misma ruta que la mencionada en la pagina web (/var/www/mipagina.es/styles).
```bash
#Nos posicionamos en la ruta de lso estilos (styles)
cd /var/www/mipagina.es/styles
# - Ubicación actual: /var/www/mipagina.es/styles

#Creamos un fichero el cual almacenara los estilos de la pagina, esto nos abrira una ventana para introducir texto, de la misma manera que sucedio con la creacion del fichero de scripts de JavaScript
sudo touch style.css
# - Ubicación actual: /var/www/mipagina.es/scripts/style.css
```

Posteriormente agregamos contenido dentro del fichero, este ha de ser código `CSS`, esto permitira a la pagina obtener un estilo, bien sea un color de fondo, efectos, fuentes de texto...

Un ejémplo sencillo de codigo `CSS` es el siguiente:
```css
/*Color de fondo en el cuerpo de la pagina*/
body {
  background-color: #fefbd8;
}

/*Color de fondo en los titulos h1*/
h1 {
  background-color: #80ced6;
}

/*Color de fonod en los divs*/
div {
  background-color: #d5f4e6;
}

/*Color de fondo en los span*/
span {
  background-color: #f18973;
}

/*Estilos cuerpo de texto*/
p {
font-family: 'Open Sans';
font-size: 14px;
color: #ccc;
line-height: 18px;
margin-bottom: 20px;
}
```
Con esto hecho, tenemos la apgina web montada enlazada con los scripts y diseño, a continuación deberemos crear un certificado, para ello instalaremos `openssl`:
