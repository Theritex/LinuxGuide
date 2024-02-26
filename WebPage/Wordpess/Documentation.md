# Documentación Worpress
<!--Documentado por Andrés Abadías (Nisamov)-->

[ ! ] Recomendación: Para obtener referencias y más apuntes, dirígase a nuestra [Documentación Apache2](https://github.com/Theritex/LinuxGuide/blob/main/WebPage/Apache2/Documentation.md).

Para este ejercicio hay que instalar Ubuntu, en este caso usaremos un `Ubuntu desktop 22.04.06`.

Tras la instalación, actualizamos la máquina con el siguiente comando:
```bash
#Actualizamos los paquetes de la maquina
sudo apt update
```
A continuación, instalamos el siguiente contenido, los cuales son las dependencias y paquetería de los programas que vamos a usar mediante el comando que aparece a continuación:
```bash
#Instalamos los reqisitos de uso
sudo apt install apache2 php8.1 php8.1-bcmath php8.1-curl php8.1-gd php8.1-mbstring php8.1-mysql php8.1-pgsql php8.1-xml php8.1-zip mariadb-server mariadb-client wget
```

Este comando nos permitirá instalar todos los paquetes en un solo comando, evitando tener que ir por fragmentos, ahorrandonos tiempo.

Usando el entorno gráfico instalamos wordpress y lo descomprimimos,
Movemos el directorio a la ruta raíz.

Hay que tener en cuenta nuestra posicion actual dentro del sistema operativo, pues si tenemos worpress descomprimido en la ruta /Desktop o /Downloads, hay que estar ubicado en esta misma ruta, o bien se deberá modificar el ruta principal del objetivo para poder hacer que el comando pueda funcionar sin problemas:
```bash
#Clonamos el repositorio descomprimido dentro de la ruta raíz con el nombre "wordpress"
sudo mv wordpress /wordpress
# - Ubicación actual: /home/user/Downlods
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
#Accedemos a la ruta /sites-aviable
cd /etc/apache2/sites-aviable
#Deshabilitamos la configuración por defecto
sudo a2dissite 000-default
# - Ubicación actual: /etc/apache2/sites-aviable
```
Recargamos el servicio apache para comprobar los cambios:
```bash
#Reiniciamos el servicios apache2
sudo service apache2 restart
```
Creamos un nuevo fichero de configuración en la misma ruta previa:
```bash
#Accedemos a la ruta en caso de habernos movido de ubicación
cd etc/apache2/sites-available/
#Creamos el fichero de configuración y accedemos a su edición
sudo nano wordpress.conf
# - Ubicación actual: /etc/apache2/sites-available/
```
Agregamos el contenido de la configuración con las rutas necesarias apra su funcionamiento:
```bash
<VirtualHost *:80>
ServerAdmin webmaster@localhost
DocumentRoot “/wordpress”
<Directory “/wordpress”>
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