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
Para poner un ejemplo de como sería la estructura basica de CSS y JavaScript:

### Estructura de directorio
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