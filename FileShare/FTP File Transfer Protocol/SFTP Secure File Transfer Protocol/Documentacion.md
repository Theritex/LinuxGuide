# Documentación FTP
<!--Documentado por Andrés Abadías (Nisamov)-->
## Teoría Previa a la Práctica
Contamos con dos tipos de encriptación, siendo estos los siguientes:
- Encriptación simétrica
- Encriptación asimétrica

Para llevar a cabo las encriptaciones deberemos crear una llave, contamos con dos tipos de llaves a crear, siendo estas, la llave `pública` o `privada`, estas mismas pueden usar una encriptación en binario o ascii, permitiéndonos elegir el tipo de encriptación durante su creación.
- Llave pública (usada para encriptar)
- Llave privada (usada para desencriptar RSA {mensaje general encriptado con RSA}-> 4096 bits)
    - Passphrase: Clave Privada

# Instalación de Dependencias:
Instalamos los programas que necesitaremos usar a continuación:
```bash
#Actualizamos los paquetes
sudo apt update
#Instalación de requisitos
sudo aptinstall ftp
sudo apt install vsftpd
sudo apt install gpg
sudo apt install ufw
```

## FTP Establecer Canal Encriptado y Seguro
Para llevar a cabo dicho protocolo, son necesarias dos maquinas, entre las cuales se va a establecer un canal, cifrado y seguro.
Ambas máquinas deberán tener una direecion ip estática, evitando así problemas en momentos de desconexión.

## Instalación
Comenzamos con la instalación de los requisitos:
```bash
#Acceso al fichero de configuración de ftp
sudo nano /etc/vsftpd.conf
```
Dentro de este fichero aparecerá contenido ya escrito, del cual, cuenta con una línea comentada, siendo esta `#Port 22`, esta misma linea deberá ser descomentada, habilitando así las conexiones por el puerto 22.

Tras este cambio, guardamos el fichero de configuración y reinciiamos el servicio `sshd`.
```bash
#Reiniciamos el servicio sshd
sudo service sshd restart
```
## Conexión Remota
Para conectarnos con el cliente, deberemos tener en cuenta su direccion IP estática, mediante el comando `sftp {ip}` podremos conectarnos a el, únicamente sabiendo las credenciales de uno de los usuarios que se almacenan en ese equipo.
```bash
#Conexión remota al equipo 192.168.102.120
sudo sftp 192.168.102.120
```

Para compartir ficheros, deberemos tener en cuenta nuestra posición actual, si estamos dentro del equipo remoto o en nuestro equipo local, para ello, deberemos estar dentro de nuestro equipo local, mediante el comando `put {fichero}`, enviamos el fichero a al equipo remoto.
```bash
#Enviamos el fichero "fichero.txt"
put fichero.txt
```


|---------------------|

## Lista Comandos uso
```bash
gpg -k                                                      Ver claves
gpg --gen-key			                                    Crear una key
gpg --symmetric -a ejercicio.txt	                        Cifrado simétrico
gpg --symmetric ejercicio.txt                               Cifrado simétrico en binario
gpg --symmetric -o ejercicio.twofish/3des ejercicio.txt     Pasar a encriptación 3des o twofish
gpg --full-generate-key                                     Creación completa de una key (elección de Bits)
```