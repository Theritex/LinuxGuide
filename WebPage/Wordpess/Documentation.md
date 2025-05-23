# Documentación Wordpress
<!--Documentado por Andrés Ruslan Abadías Otal (Nisamov)-->
> Documentado por Andrés Ruslan Abadías Otal | [Nisamov](https://github.com/Nisamov)

[ ! ] Recomendación: Para obtener referencias y más apuntes, dirígase a nuestra [Documentación Apache2](https://github.com/Theritex/LinuxGuide/blob/main/WebPage/Apache2/Documentation.md).

Para este ejercicio hay que instalar Ubuntu, en este caso usaremos un `Ubuntu desktop 22.04.04s`.

Tras la instalación, actualizamos la máquina con el siguiente comando:
```bash
#Actualizamos los paquetes de la maquina
sudo apt update
```
A continuación upgradeamos la máquina (recomendado):
```bash
#Upgradeamos la maquina
sudo apt upgrade
```

A continuación, instalamos el siguiente contenido, los cuales son las dependencias y paquetería de los programas que vamos a usar mediante el comando que aparece a continuación:
```bash
#Instalamos los reqisitos de uso
sudo apt install apache2 php8.1 php8.1-bcmath php8.1-curl php8.1-gd php8.1-mbstring php8.1-mysql php8.1-pgsql php8.1-xml php8.1-zip mariadb-server mariadb-client wget
```
De otro modo, también podemos instalar los requisitos mediante los siguientes comandos (instalación por secciones):
```bash
sudo apt install apache2
sudo apt install php7.4
sudo apt install wget
sudo apt install mariadb-server
sudo apt install mariadb-client
sudo apt install php8.1
sudo apt install php8.1-mysql
sudo apt install php8.1-curl
sudo apt install php8.1-gd
sudo apt install php8.1-bcmath
sudo apt install php8.1-cgi
sudo apt install php8.1-ldap
sudo apt install php8.1-mbstring
sudo apt install php8.1-xml
sudo apt install php8.1-soap
sudo apt install php8.1-xsl
sudo apt install php8.1-zip
#En caso de ser necesario, instalar:
sudo apt install libapache2-mod-php php-mysql -y
```
Este comando nos permitirá instalar todos los paquetes en un solo comando, evitando tener que ir por fragmentos, ahorrandonos tiempo.

Usando el entorno gráfico instalamos wordpress y lo descomprimimos,
Movemos el directorio a la ruta raíz.

Hay que tener en cuenta nuestra posicion actual dentro del sistema operativo, pues si tenemos worpress descomprimido en la ruta /Desktop o /Downloads, hay que estar ubicado en esta misma ruta, o bien se deberá modificar el ruta principal del objetivo para poder hacer que el comando pueda funcionar sin problemas:
```bash
#Clonamos el repositorio descomprimido dentro de la ruta raíz con el nombre "wordpress"
sudo mv wordpress /wordpress
# - Ubicación actual: /home/user/Downloads
```
Posteriormente a los pasos previos, hay que otorgar permisos a los directorios con los que vamos a trabajar:
```bash
#Accedemos a la ruta de la clonacion del directorio worpress
cd /wordpress
#Otorgamos permisos dentro del repositorio
sudo chown www-data:www-data .
sudo chown www-data:www-data -R *
# - Ubicación actual: /wordpress
```
Tras otorgar los pemisos necesarios, cambiamos la ruta por defecto de apache2.
Deshabilitamos el fichero de configuración de apache2:
```bash
#Accedemos a la ruta /sites-available
cd /etc/apache2/sites-available
#Deshabilitamos la configuración por defecto
sudo a2dissite 000-default
# - Ubicación actual: /etc/apache2/sites-available
```
Recargamos el servicio apache para comprobar los cambios:
```bash
#Reiniciamos el servicios apache2
sudo service apache2 restart
```
Creamos un nuevo fichero de configuración en la misma ruta previa:
```bash
#Accedemos a la ruta en caso de habernos movido de ubicación
cd /etc/apache2/sites-available/
#Creamos el fichero de configuración y accedemos a su edición
sudo nano wordpress.conf
# - Ubicación actual: /etc/apache2/sites-available/
```
Agregamos el contenido de la configuración con las rutas necesarias apra su funcionamiento:
```bash
<VirtualHost *:80>
  ServerAdmin webmaster@localhost
  DocumentRoot /wordpress
  <Directory /wordpress>
  DirectoryIndex index.php
  AllowOverride All
  Require all granted
  </Directory>
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
# - Ubicación actual: /etc/apache2/sites-available/wordpress.conf
```
Tras guardar y aplicar los cambios, habilitamos nuestra página:
```bash
#Habilitamos la pagina wordpress
sudo a2ensite wordpress
# - Ubicación actual: /etc/apache2/sites-available
```
Recargamos nuevamente la configuración del servicio apache2:
```bash
#Reniciamos el servicio apache2
sudo systemctl restart apache2
# - Ubicación actual: /etc/apache2/sites-available
```
Tras aplicar los cambios y reiniciar el servicio, procedemos a crear las bases de datos para la página, comenzando con la ejecucion del gestor de bases de datos:
```bash
#Ejecución del gestor de bases de datos BDD
sudo mysql -u root -p
```
Modificamos la información del `usuario root`:
```bash
update mysql.user set plugin=’mysql_native_password’ WHERE user=’root’;
```
Leemos la lista de privilegios:
```bash
flush privileges;
```
Tras todos los pasos anteriores, procedemos a la creación de una base de datos para nuestra página:
```bash
create database wordpress;
```
Tras la creación de la misma, salimos del programa:
```bash
exit;
```
Finalmente procedemos a la instalación segura, la cual permite
ajustar una serie de parámetros básicos de seguridad con el siguiente comando:
```bash
sudo mysql_secure_installation
```
Durante la instalación, realizará varias preguntas, ae stas contestamos que sí, en la última pregunta, `solicitará una contraseña`, esta misma será para `mysql`, guarda esa contraseña en un papel o un documento de forma que no la pierdas.

Tras el proceso comleto es posible que nos pida crear una abse de datos `wp-config.php`, en el interior de esta base de datos hay que agregar la siguiente estructura:
```php
<?php
// ** Database settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** Database username */
define( 'DB_USER', 'root' );

/** Database password */
define( 'DB_PASSWORD', 'andres' );

/** Database hostname */
define( 'DB_HOST', 'localhost' );

/** Database charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8mb4' );

/** The database collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );

define( 'AUTH_KEY',         ']&eKuM6^mw^;,|:[-p?_[XJBkMr<E8&cJGR(.k|1v%bl-Q7szq|ipsgb411e4U}G' );
define( 'SECURE_AUTH_KEY',  '7|6f^<J>F?4RR%}%k(IY;s)cqY%M4cW*Yp?qXroB[(jf9Zwfzx|r$G{$J3r86qf<' );
define( 'LOGGED_IN_KEY',    ')xhXtUA|AkftoP@R-dPm|iI{?!i7^T>s~/*(@{,naV#9i kZ1pCK]|(31K5}u*J7' );
define( 'NONCE_KEY',        '4$pu@PCrf|dBg^4K_Q>Iqaf| 5St~GU,n<#nl`2PghIU G55z/N]lcl%l68kfHd-' );
define( 'AUTH_SALT',        'MD.SxW+}g`q3Ub}LN>!|,P<+Ya5QFR(vd1H2kE&U(|cl-hGUPq<I${#Ahaxl*H4J' );
define( 'SECURE_AUTH_SALT', 'E^p{[?C4}/0ZG:}7V)OBakc~cL]M=}X.=:12sq `XM|O+]3](54sZZamq9g&/woy' );
define( 'LOGGED_IN_SALT',   '4@p34O)[p)X}N KVsWu_l5<oaXRH>U/{XR,?d8;vRyI=,(Z8vNy%}yG$uH8)ht|9' );
define( 'NONCE_SALT',       'a2u!6q:UrqN96JM,tWy-W==Z&[:!&0pNrbiC{XQ0DdG%! ip^N<F1&M0 Gx|AsE(' );

/**#@-*/
$table_prefix = 'wp_';
define( 'WP_DEBUG', false );

/** Absolute path to the WordPress directory. */
if ( ! defined( 'ABSPATH' ) ) {
	define( 'ABSPATH', __DIR__ . '/' );
}

/** Sets up WordPress vars and included files. */
require_once ABSPATH . 'wp-settings.php';
```
Tras llevar a cabo todas las configuraciones necesarias, procederemos a la instalación completa de la pagina dentro del navegador y seguiremos procediendo hasta completar todo el servicio.
