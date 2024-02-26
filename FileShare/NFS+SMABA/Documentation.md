# Documentación SAMBA
## Instalación
Instalamos el servicio samba como root:
```bash
sudo apt install samba
```
## Creación de directorios compartidos
Tras la instalación, procedemos a crear un directorio, en este caso dentro de la ruta `/home/Documents`:
```bash
#Creamos los directorios que compartiremos en un futuro
mkdir carpeta1
mkdir carpeta2
# - Ubicación actual: /home/Documents
```
Otorgamos permisos necesarios a los directorios creados:
```bash
#Agregamos los maximos permisos a todas las carpetas que contengan "carpeta" sin importar su continuación, permitiendo englobar tanto a carpeta1 como a carpeta2
chmod 777 carpeta*
# - Ubicación actual: /home/Documents
```
## Edición fichero de configuración SAMBA
Editamos el fichero de configuración de samba con root:
```bash
sudo nano /etc/samba/smb.conf
```
En el interior del fichero de configuración crearemos una estructura similar a la siguiente:
```bash
#Nombre carpeta1
[nombreCarpeta]
#Permisos de la carpeta
    permiso1
    permiso2
    permiso3
    permiso4
#Nombre carpeta2
[nomrbeCarpeta2]
#Permisos de la carpeta
    permiso1
    permiso2
    permiso3
    permiso4
```
Ese ejemplo permite mostrar de una manera sencilla la siguiente estructura a la cual está basada:
```bash
[carpeta1]
    comment = Comentario de carpeta pública
	path = /home/Documents/carpeta1
	browseable = yes
	guest ok = yes
#   Similar a guest ok
	public = yes
	writeable = yes
#   Pemrisos de directorios (777)
	directory mask = 0…
#   Pemrisos de ficheros (777)
	create mask = 0…
#   Usuarios permitidos
	valid users = usuario1, usuario2
#   Se le asigna automáticamente una lista de lectura
	write list = usuario1, usuario2
	read list = usuario2
#   Oculta todo el contenido que cuente con una de estas extensiones
	veto files = /*.avi/*.mp3/*.pdf/
#   Elimina todo el contenido que clasificado como vetar en los veto files
	delete veto files = yes
```
## Reinicio de servicios
Reiniciamos y analizamos los servicios:
```bash
#Reiniciamos el servicio nmbd
service nmbd restart
#Reiniciamos el servicio smbd
service smbd restart
#Comprobamos el estado del servicio nmbd
service nmbd status
```
Si es requerido agregar un usuario al cual otorgarle los permisos de acceso, usaremos el siguiente comando:
```bash
sudo adduser usuario
```

# Documentación NFS
## Instalación
Instalamos los paquetes necesarios para su correcto funcionamiento:
```bash
#Paquete necesario para el cliente
sudo apt install nfs-common
#Paquete necesario para los servidores
sudo apt install nfs-kernel-server
```
## Edición fichero configuración NFS
Abrimos y editamos el fichero de configuración al cual otorgarle permisos:
```bash
#Abrimos y editamos el fichero /etc/exports
sudo nano /etc/exports
```
Dentro de este fichero habrá que tener en cuenta una estructura la cual segur para poder asignar tantos directorios como es requerido:
Estructura base:
```bash
direccion/carpeta/en/servidor direccion.ip.servidor(permiso1,permiso2,permiso3)
```
Tras ver la estructura base, puedes identificar la estructua a agregar dentro de `/etc/exports`:
```bash
/home/user/carpeta1 40.0.0.0/8(rw,sync,subtree_check,no_root_squash)
/home/user/carpeta2 40.0.0.0/8(rw,sync,subtree_check,no_root_squash)
/home/user/carpeta3 *(ro,sync,subtree_check,root_squash)
```
Para poder configurar la linea previa, es necesario conocer los permisos disponibles:
```bash
rw			        Lectura y escritura
ro                  Solo lectura
nfs-kernel-server   restart
root_squash         Degradar root (quita permisos a los root)		
no_root_squash      No degradar root						
sync        		Sincronización						
async		        No sincronizar						
subtree_check		Comprobar de donde procede				
no_subtree_check	No comprobar de donde procede
wdelay		        Retarde la escritura en disco para permitir solicitudes consignadas
no_wdelay		    Desactiva la opción predeterminada wdelay
```
## Instalación de dependencias
Dentro de la maquina cliente es necesario instalar `rpcbind`, debido a que es una dependencia de nfs-kernel-server:
```bash
sudo apt install rpcbind
```
## Montaje de directorios
Tras rpcbin en la maquina cliente, montamos el directorio dentro de la ruta que se quiera, mientras usamos la maquina cliente:
```bash
sudo mount IP:/nombreCarpeta /rutaLocalEnLaQueMontarla
```