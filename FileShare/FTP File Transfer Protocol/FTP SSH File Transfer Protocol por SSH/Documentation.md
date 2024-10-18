# Documentación FTP por SSH
<!--Documentado por Andrés Ruslan Abadías Otal (Nisamov)-->

## Teoría Previa a la Práctica
Para establecer una conexión segura a un equipo remoto, se deberá saber las credenciales, siendo estas, usuario y contraseña para su acceso remoto.

Mediante este protocolo, es posible conectarse tanto a sistemas windows como a dispositivos GNU/Linux.

La conexión por ssh es un protocolo que permite la interacción entre máquinas utilizando una línea de comandos de forma segura
`Puerto 22 de TCP -> IP -> Red`

El `servicio sshd` es el demonio (daemon) que queda esperando peticiones al puerto 22, iendo este puerto, el puerto de conexión por defecto de tcp
- SSH = “Secure SHell”

## Comandos a tener en cuenta
### Comandos:
```bash
# Iniciar sesión como usuario
ssh {nombre-usuario}@{IP}
#Enviar archivo.txt
put archivo.txt

# Fichero de configuración de ssh
nano /etc/ssh/sshd_config
# Recargar configuración de sshd (ssh)
service sshd restart

# Conectarse por SSH a un sistema remoto
ssh [user]@[ip]
└─ sudo apt install openssh-server
```
### Ejemplo:
```bash
ssh asir@192.168.115.205
put controller.exe
```
## Práctica FTP - SSH
Para comenzar con la práctica, deberemos acceder el fichero de configuración `vsftpd.conf` mediante el siguiente comando:
```bash
nano /etc/vsftpd.conf
```
Dentro de este fichero escribiremos el siguiente contenido:
```bash
write_enable=YES
listen port=3000
ftp_data_port=4000
```
Esto permite que nos conectemos por el puerto 3000 y enviemos los datos por él 4000 mediante ftp.


### Envío de archivos:
Este comando permite loguearnos como `usuario`:
```bash
scp {/home/dirección/nuestro/archivo.extensión} {usuario}@{IP:/home/ubicación/archivo/destino}
```
Siendo así un ejemplo práctico:
```bash
#En este ejemplo nos logueamos como user1 enviando nuestor fichero a su directorio /desktop
scp /home/user/desktop/file.txt user1@192.168.102.111:/home/user1/desktop/
```

# Ejemplo de uso:
```bash
# Encriptar un fichero
# Se puede sustituir el tipo de encriptación (-aes-256-cbc) mediante openssl help
openssl genrsa -aes-256-cbc -out nombre-privada.key 4096
# Crear key de privada a clave publica
openssl rsa -in nombre-privada.key -pubout > nombre-pública.key
# Conexion remota a equipo 192.168.115.205
scp /home/user/nombre-publica.key 192.168.115.205@user:/home/user/
# Desencriptacion de clave
openssl rsautl --decrypt -inkey claveprivada.key -in encriptado.enc
```