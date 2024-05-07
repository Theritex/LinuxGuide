# Proxy Server - Squid
<!--Documentado por Andrés Abadías (Nisamov)-->
## Introduccion:

Squid es uno de los proxy cachés más utilizados hoy en día y tiene como principal función atender a las
peticiones HTTP de una red interna. Es un popular programa de software libre que implementa un
servidor proxy y un dominio para caché de páginas web, publicado bajo licencia GPL.

Tiene una amplia variedad de utilidades, desde acelerar un servidor web, guardando en caché peticiones
repetidas a DNS y otras búsquedas para un grupo de gente que comparte recursos de la red, hasta caché de
web, además de añadir seguridad filtrando el tráfico.

Está especialmente diseñado para ejecutarse bajo entornos tipo Unix.
Squid ha sido desarrollado durante muchos años y se le considera muy completo y robusto. Aunque
orientado principalmente a HTTP y FTP es compatible con otros protocolos.

## Principales características:

Squid posee las siguientes características:
- Proxy y Memoria Caché de HTTP, FTP, y otras direcciones: Squid proporciona un servicio
de Proxy que soporta peticiones HTTP, HTTPS y FTP a equipos que necesitan acceder a
Internet y a su vez provee la funcionalidad de caché especializado en el que se almacena de
forma local las páginas consultadas recientemente por los usuarios. De esta forma, incrementa
la rapidez de acceso a los servidores de la Web y FTP que se encuentra fuera de la red interna.
- Proxy por SSL: Squid también es compatible con SSL (Secure Socket Layer) con el que
también acelera las transacciones cifradas, y es capaz de ser configurado con amplios controles
de acceso sobre las peticiones de usuarios.

- Jerarquías de caché: Squid puede formar parte de una jerarquía de caches. Varios proxys
trabajan conjuntamente sirviendo las peticiones de las páginas. Un navegador solicita siempre
las páginas a un solo proxy, si éste no tiene la página en la caché hace peticiones a sus
hermanos, que si tampoco las tienen las hacen a su/s padre/s. Estas peticiones se pueden realizar
mediante dos protocolos: HTTP e ICMP.

- ICP, HTCP, CARP, memoria caché digests: Squid sigue los protocolos ICP, HTCP, CARP y
memoria caché digests que tienen como objetivo permitir a un proxy "preguntar" a otros proxys
caché si poseen almacenado un recurso determinado.

- Caché transparente: Squid puede ser configurado para ser usado como proxy transparente por
lo que las conexiones son enrutadas dentro del proxy sin configuración por parte del cliente, y
habitualmente sin que el propio cliente conozca de su existencia. De forma predeterminada,
- Squid utiliza el puerto 3128 para atender peticiones, pero se puede especificar que lo haga en
cualquier otro puerto disponible o que lo haga en varios puertos disponibles a la vez.

- WCCP: A partir de la versión 2.3 Squid implementa WCCP (Web Cache Control Protocol).

- Permite interceptar y redirigir el tráfico que recibe un router hacia uno o más proxys caché,
haciendo control de la conectividad de éstos. Además permite que uno de los proxys caché
designado pueda determinar puede distribuir el tráfico redirigido a lo largo de todo el array de
proxys caché.

- Control de acceso: Ofrece la posibilidad de establecer reglas de control de acceso. Esto
permite establecer políticas de acceso en forma centralizada, simplificando la administración de
una red.

- Aceleración de servidores HTTP: Cuando un usuario hace petición hacia un objeto en
Internet, éste es almacenado en la caché, si otro usuario hace petición hacia el mismo objeto, y
éste no ha sufrido ninguna modificación desde que lo accedió el usuario anterior, Squid
mostrará lo que ya se encuentra en la caché en lugar de volver a descargar desde Internet. Esta
función permite navegar rápidamente cuando los objetos ya están en la guarida y además
optimiza enormemente la utilización del ancho de banda.

- SNMP: Squid permite activar el protocolo SNMP, éste proporciona un método simple de
administración de red, que permite supervisar, analizar y comunicar información de estado
entre una gran variedad de máquinas, pudiendo detectar problemas y proporcionar mensajes de
estados.

- Caché de resolución DNS: Squid está compuesto también por el programa dnsserver, que se
encarga de la búsqueda de nombres de dominio. Cuando Squid se ejecuta, produce un número
configurable de procesos dnsserver, y cada uno de ellos realiza su propia búsqueda en DNS. De
esta forma, se reduce la cantidad de tiempo que la caché debe esperar a estas búsquedas DNS.

## Esquema de simulación:

PC-PT (Ubuntu Desktop) – `172.30.1.10`

Server-PT (Ubuntu Proxy Server) – `172.30.0.1`

`[Ubuntu Desktop]----[Switch]----[Ubuntu Proxy Server]----[Internet]`

## Requisitos:

El software del servidor requiere la instalación, entre otros, del paquete squid (servidor).

Para llevar a cabo este proceso completamente, es necesario instalar lo siguiente dentro del equipo servidor (Ubuntu Proxy Server [172.30.0.1]):
- Apache2
- Squid
- Squidguard

```sh
sudo apt update
sudo apt install apache2
sudo apt install squid
sudo apt install squidguard
```

## Revisión de Conexión

Antes de continuar, es necesario revisar si hay conexión entre los equipos, de lo contrario, no funcionará lo que se haga a continuación.

## Fichero Configuración Squid:

El archivo de configuración es **squid.conf** situado en el directorio **/etc/squid/** resultante de la
instalación. La configuración del servicio será pues modificando, eliminando o añadiendo nuevas
opciones a este archivo. Por tanto se recomienda que antes de modificar nada en su interior se
realice una copia de seguridad para poder restaurarlo a su estado original en caso de ser necesario.
La ruta completa es: **/etc/squid/squid.conf**

## Configuración del Proxy en Ubuntu Server:

Descomentar los parámetros explicados en el apartado «2. Configuración del Servidor
como Proxy-cache», asegurándose de que tienen el valor indicado.

Permitir que cualquier ordenador de nuestra red interna tenga acceso a internet. Si existen
otras redes preconfiguradas para poder acceder a internet, eliminar su permiso.

El proxy sólo debe permitir acceder al puerto http (80) y al https (443), pero no al ftp (21).
Si existen otros puertos preconfigurados para poder acceder a internet, eliminar su permiso.
(Comentar los puertos no indicados), dejando el siguiente resultado:
```conf
acl localnet src 0.0.0.1-0.255.255.255
acl localnet src 10.0.0.0/8
acl localnet src 100.64.0.0/10
acl localnet src 169.254.0.0/16
acl localnet src 172.16.0.0/16
acl localnet src 172.30.0.0/16            # Red local 172.30.0.0/16
acl localnet src 192.168.0.0/16
acl localnet src fc00::/7
acl localnet src fe80::/10

acl SSL_ports port 443
acl Safe_ports port 80
#acl Safe_ports port 21
acl Safe_ports port 443
#acl Safe_ports port 70
#acl Safe_ports port 210
#acl Safe_ports port 1025-65535
#acl Safe_ports port 280
#acl Safe_ports port 488
#acl Safe_ports port 591
#acl Safe_ports port 777
```


Reincia el servicio para aplicar los cambios, depende de la distribución usada, el comando para reiniciar el servicio puede variar:
Distribuciones Debian/Ubuntu:
`service squid restart`/`service squid3 restart`
En algunos casos, los servicios se reinician mediante el sitema init, es posible que puedas reiniciarlo mediante este servicio:
`/etc/init.d/squid restart`
Si no es viable ninguna opcion semejante, lo más seguro es apagar y volver a encender el equipo.

## Configuración del Proxy en Ubuntu Desktop - Sistema Operativo y Navegador

Configurar manualmente el proxy de sistema en Ubuntu Desktop y modificar la configuración del navegador web de forma que se conecte a internet a través de este proxy de sistema.
Conseguir acceder a dos sitios web de internet desde el navegador web de la máquina cliente.

Para llevar esto a cabo hay que realizar algunos ajustes en el proxy del Ubuntu Desktop, estableciéndolo en `configuración manual` y posteriormente, dentro de los parámetros indicados, hay que agregar lo siguiente:
```
ip: 172.30.0.1 (La dirección Ip del Servidor)
Puerto: 3128
```

## Autenticación de Conexión por Usuario

A continuación configuraremos el servidor SQUID para habilitar el acceso a internet sólo a tres usuarios definidos por usted. En el nombre de estos usuarios deberá constar las iniciales de uno de los miembros del grupo, por ejemplo en este caso serían: abg1, abg2 y abg3.

Crear tres usuarios:
Utilizamos el comando **-c** para crear una lista nueva de usuarios
Usaremos el nombre del usuario como contraseña **(user:abg1 – passwd: abg1)**
```
sudo htpasswd -c /etc/squid/passwd abg1
sudo htpasswd /etc/squid/passwd abg2
sudo htpasswd /etc/squid/passwd abg3
```
Permitir a squid leer el archivo de usuarios:
```
sudo chmod 400 /etc/squid/passwd
sudo chown proxy /etc/squid/passwd
```
En el archivo de configuración de SQUID modificar los siguientes comandos:
```
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Squid proxy-caching web server
auth_param basic casesensitive off
```
Finalmente modificar el acceso de los usuarios de nuestro proxy para dar sólo permiso y pedir autenticación a los usuarios creados anteriormente.
Se deben añadir las siguientes líneas al archivo de configuración **squid.conf**:
```
acl ncsa_users proxy_auth REQUIRED
http_access allow ncsa_users
```


## Posibles Preguntas sobre el Programa

**¿Es posible saber si ha habido intentos de acceso no autorizado de algún usuario (credenciales mal
escritas o erróneas)?:**

Si, mediante los ficheros cache.log(Registro general de avisos y errores, aquí es posible ver
accesos no autorizados) y access.log(Registro de acceso permitido por parte de dispositivos cliente
conectados).

La ruta de estos ficheros es: **/var/log/squid/access.log** y **/var/log/squid/cache.log**