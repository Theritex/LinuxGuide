<!--Esto es un modelo, está en edicion-->
NGINX
sudo su (Acceso como superusuario)
apt install nginx
apt install apache2
cd /var/www
mkdir mipagina.es
chmod 777 mipagina.es
cd mipagina.es
:: Nos logueamos en duckdns.org
nano index.html
Estructura básica html5:

<!DOCTYPE html>
<html lang="en">
<head>
    		<meta charset="UTF-8">
    		<meta name="viewport" content="width=device-width, initial-scale=1.0">
    		<title>Mi Página</title>
</head>
<body>
    			<p>Contenido cuerpo de página</p>
</body>
</html>


cd /etc/nginx
ls
cd sites-available
ls
cp default mipagina
nano mipagina

server {
listen 80;
root /var/www/mipagina.es;
index index.html index.htm;
server_name mipagina.duckdns.org www.mipagina.duckdns.org;
location / {
try_files $uri $uri/ =404;
}
}


ln -s /etc/nginx/sites-available/mipagina.es /etc/nginx/sites-enabled/mipagina.es (Creamos un enlace simbólico)
nano /etc/hosts

127.0.0.1       localhost
127.0.1.1       smr
40.0.0.2        Index.es        www.Index.es
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters


service nginx restart
service nginx status
mipagina.es.duckdns.org (Búsqueda en firefox)
www.mipagina.es.duckdns.org (Búsqueda en firefox)
apt install openssl
cd /etc/nginx
mkdir ssl
chmod 700 ssl
openssl genpkey -algorithm RSA -out /etc/nginx/ssl/clave_privada.key -out /etc/nginx/ssl/solicitud.csr
cd ssl
ls (comprobación de la creación de “clave_privada.key” y “solicitud.csr”)
openssl x509 -req -days 365  -in /etc/nginx/ssl/solicitud.csr -signkey /etc/nginx/ssl/clave_rpivada.key -out /etc/nginx//ssl/certificado.crt
ls (comprobación de la creación de “certificado.crt”)
nano /etc/nginx/sites-enabled/mipagina

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

