# Documentación FTP por SSH

## Teoría Previa a la Práctica
Para establecer una conexión segura a un equipo remoto, se deberá saber las credenciales, siendo estas, usuario y contraseña para su acceso remoto.

Mediante este protocolo, es posible conectarse tanto a sistemas windows como a dispositivos GNU/Linux.

La conexión por ssh es un protocolo que permite la interacción entre máquinas utilizando una línea de comandos de forma segura

`Puerto 22 de TCP -> IP -> Red`

El `servicio sshd` es el demonio (daemon) que queda esperando peticiones al puerto 22, iendo este puerto, el puerto de conexión por defecto de tcp

- SSH = “Secure SHell”

## Práctica FTP - SSH

nano /etc/ssh/sshd_config	>>Fichero de configuración de ssh
service sshd restart	>>Recargar configuración de sshd (ssh)

ssh [user]@[ip]		>>Conectarse por SSH a un sistema remoto
└─ sudo apt install openssh-server

nano /etc/vsftpd.conf

write_enable=YES
listen port=3000
ftp_data_port=4000
esto hace que nos conectemos por el puerto 3000 pero enviemos los datos por él 4000 mediante ftp

Comandos:
ssh {nombre-usuario}@{IP} >>Iniciar sesión como usuario
put archivo.txt >>Enviar archivo.txt


Ejemplo:
ssh asir@192.168.115.205
put controller.exe


Envío de archivos:
Este comando permite loguearnos como asir
scp {/home/dirección/nuestro/archivo.extensión} {usuario}@{IP:/home/ubicación/archivo/destino}


Ejemplo:
openssl genrsa -aes-256-cbc -out nombre-privada.key 4096 >>Se puede sustituir el tipo de encriptación (-aes-256-cbc) mediante openssl help
openssl rsa -in nombre-privada.key -pubout > nombre-pública.key
scp /home/user/nombre-publica.key 192.168.115.205@user:/home/user/


openssl rsautl --decrypt -inkey claveprivada.key -in encriptado.enc > desencriptado
