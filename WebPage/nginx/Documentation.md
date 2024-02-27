# Documentación NGINX
<!--Documentado por Andrés Abadías (Nisamov)-->
[ ! ] Recomendación: Se recomienda revisar el contenido de la [Documentación de apache2](https://github.com/Theritex/LinuxGuide/blob/main/WebPage/Apache2/Documentation.md), pues contiene información que puede ser útil a la hora de llevar a cabo este servicio.

Instalamos los servicios necesarios para poder continuar:
```bash
sudo apt install nginx
sudo apt install apache2
```
Accedemos a la ruta `/var/www` y creamos un directorio en su interior:
```bash
#Accedemos a la ruta
cd /var/www
#Creamos un directorio en su interior
mkdir mipagina.es
#Le otorgamos permisos
sudo chmod 777 mipagina.es
#Accedemos al directorio creado
cd mipagina.es
#Creamos un index.html, que será nuestra página principal
nano index.html
```
Tras crear y abrir el fichero `index.html`, agregamos la estructura basica de una página web:
```html
<!DOCTYPE html>
<html lang="es">
<head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>Mi Página</title>
</head>
	<body>
    	<p>Contenido cuerpo de página</p>
	</body>
</html>
```
En internet, iremos a la pagina duckdns.org y nos adaremos de alta, registrando el nombre de la página, esot nos permitirá ofrecer serviicos mediante una direccion que nos proporcionará duckdns.

Accedemos a la ruta de nginx `/etc/nginx/sites-available` y creamos el fichero de configuración:
```bash
#Accedemos a la ruta
cd /etc/nginx/sites-available
#Copiamos el fichero y lo renombramos como mipagina
sudo cp default mipagina
#Editamos el fichero
sudo nano mipagina
```
Dentro del fichero previo, agregarmeos el siguiente contenido:
```bash
server {
listen 80;
root /var/www/mipagina.es;
index index.html index.htm;
#Aquí hacemos referencia a la direccion con la que nos vincularemos dentro de duckDNS
server_name mipagina.duckdns.org www.mipagina.duckdns.org;
location / {
try_files $uri $uri/ =404;
}
}
```
Posteriormente hacemos uin enlace simbólico
```bash
sudo ln -s /etc/nginx/sites-available/mipagina.es /etc/nginx/sites-enabled/mipagina.es 
```
Editamos el  fichero `/ect/hosts`:
```bash
sudo nano /etc/hosts
```
Dentro de este ficheroi, tendremos que tener la siguiente estructura:
```bash
127.0.0.1       localhost
127.0.1.1       theritex
40.0.0.2        index.es        www.index.es
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```
Reiniciamos el servicio nginx:
```bash
#Reiniciamos el servicio
service nginx restart
#Comprobamos su estado
service nginx status
```
Para comprobar su correcto funcionamiento, buscaremos nuestra página con el enlace que hayamos creado, dependiendo de la direccion registrada variará este mismo:
```bash
#Realizamos las busquedas de la pagina en internet
mipagina.es.duckdns.org
www.mipagina.es.duckdns.org
```
Para continuar con la página, instalaremos más servicios, en este caso será openssl:
```bash
#Instalamos el servicio openssl
sudo apt install openssl
```
Accede a la ruta `/etc/nginx` y crea un directorio donde almacenar las claves:
```bash
#Accedemos a la ruta previa
cd /etc/nginx
#Creamos un directorio
mkdir ssl
#Le otrogamos permisos al directorio previo
chmod 700 ssl
```
Ahora procedemos a crear las llaves:
```bash
#Creamos las claves y solicitud dentro de /etc/nginx/ssl
openssl genpkey -algorithm RSA -out /etc/nginx/ssl/clave_privada.key -out /etc/nginx/ssl/solicitud.csr
#Accedemos a la ruta ssl y comprobamos el resultado
cd ssl
#Comprobación de la creación de “clave_privada.key” y “solicitud.csr”
ls -al
#Genemamos un certificado dentro del mismo directorio
openssl x509 -req -days 365  -in /etc/nginx/ssl/solicitud.csr -signkey /etc/nginx/ssl/clave_rpivada.key -out /etc/nginx/ssl/certificado.crt
#Comprobación de la creación de “certificado.crt”
ls -al
```
Posteriormente nos dirigimos a la ruta `/etc/nginx/sites-enabled/mipagina` para editarla y agregar más contenido:
```bash
nano /etc/nginx/sites-enabled/mipagina
```
Dentro de esta ruta agregaremos el siguiente contenido:
```bash
server {
listen 80;
root /var/www/mipagina.es;
index index.html index.htm;
server_name mipagina.duckdns.org www.mipagina.duckdns.org;
location / {
try_files $uri $uri/ =404;
}
}

server {
listen 443 ssl;
server_name mipagina.duckdns.org www.mipagina.duckdns.org;
root /var/www/mipagina.es;
index index.html index.htm;
ssl_certificate /etc/nginx/ssl/certificado.crt;
ssl_certificate_key /etc/nginx/ssl/clave_privada.key
location / {
try_files $uri $uri/ =404;
}
}
```
Este contenido nos permitirá estbalecer conexión con la dirección `duckdns`, así como especificaremos la ruta de los ficheros, siendo estos los certificados y claves requeridas para una conexión segura.

Por último reiniciaremos los siguiente servicios y si el procedimiento se ha llevado a cabo, su página estaría lista para ser vista desde cualquier dispositivo.
```bash
#Reiniciamos todos los servicios
service nginx restart
service apache2 restart
service openssl restart
#Comprobamos el estado de cada uno de estos servicios
service nginx status
service apache2 status
service openssl status
```