# Documentación OpenVPN
<!--Documentado por Andrés Abadías (Nisamov)-->
> Documentado por Andrés Ruslan Abadías Otal | [Nisamov](https://github.com/Nisamov)

Una VPN crea una camino virtual a través de internet para conectarse sin una conexión directa a una red.

Para poder comenzar a usar este servicio, deberemos instalar los siguientes requisitos:
```bash
#Actualiza la paquetería del  sistema
sudo apt update
#Instala las dependecias
sudo apt install vsftpd
sudo apt install openssh-server
sudo apt install ftp
sudo apt install openssl
sudo apt install git
sudo apt install openvpn
```
Para continuar, deberemos clonar un repositorio de GitHub (Viejo):
```bash
git clone https://github.com/OpenVPN/easy-rsa-old
```
Hacemos una copia del repositorio y la guardamos como un backup, permitiendonos vovler a comenzar desde un punto de partida en caso de hacer algo mal.
```bash
cp easy-rsa-old  /home/Documents/easy-rsa-old-bk
```
A continuación, establecemos la red en estática con ayuda del netplan:
```bash
#Accede al fichero de configuración
sudo nano /etc/netplan/00-network-manager-all.yml
```
```bash
network:
	version: 2
	renderer: networkd (por defecto está NetworkManager)
	ethernets:
	enp0s3:
	  dhcp4: no
	  addresses: [192.168.10.10/24]
	  gateway4: 192.168.10.1
	    nameservers:
	    addresses: [8.8.8.8]
```
Aplicamos los cambios de red:
```bash
sudo netplan apply
```
Tras aplicar la configuración de red, moveremos el repositorio local `2.0` de forma recursiva, excluyendo las capas que lo guardan, renombrándolo como `easy-rsa` dentro de la misma dirección (debería estar situado dentro del escritorio).
```bash
cp -r ./easy-rsa-old/easy-rsa/2.0/ easy-rsa
```
Estos datos te informarán sobre el contenido del repositorio renombrado:
```
> build-ca			Crear certificado autorizado
> build-dh			Diff hellman (encripta todo el trayecto de comunicación)
> build-key-server	Crea llaves de certificados de servidor
> build-key			Crea llaves de certificado de cliente
> vars				Archivo donde se indica ubicación, cliente (por defecto Estados Unidos - San Francisco)
```
En el interior del fichero `easy-rsa/vars` agregaremos el siguiente contenido, el cual nos permitirá agregar valores diferentes a cada linea.
```bash
#Editamos el fichero vars
sudo nano  vars
```
Contenido en el interior del fichero `var`:
```bash
export KEY_COUNTRY=”Country”
export KEY_PROVINCE=”Province”
export KEY_CITY=”City”
export KEY_ORG=”Key”
export KEY_EMAIL=”email@dom.x”
export KEY EMAIL=”email@host.domain”
export KEY_CN=key
export KEY_NAME=key_name
export KEY_OU=Key_Informativa
export PKCS11_MODULE_PATH=DNI Electrónico
export PKCS11_PIN=1234
```

Crear VPN:
```bash
mv openssl.1.0.0.cnf openssl.cnf
```
Detalles certificado
```bash
. ./vars					>>Propiedades de certificado
./clean-all					>>Borrar certificados creados
./build-ca					>>Crear certificado “ca.crt” dentro de /keys
./build-key-server 			>>Servidor
./build-dh					>>Crear el dh dentro de /keys
```

Archivo Configuración VPN:
```bash
# Accedemos a la ruta - ejemplos de configuracion openvpn
cd /usr/share/doc/openvpn/examples/sample-config-files
# Copiamos el fichero comprimido del servidor y lo movemos a la siguiente ruta
cp server.conf.gz /home/user/easy–rsa/keys
# Dentro de la ruta previa, accedemos al directorio
cd /home/user/easy-rsa/keys
# Decomprimimos en .gz - Descomrpimir y transformarlo en un archivo legible
gunzip server.conf.gz
# Copiamos el fichero por ruta absoluta a la siguiente ruta
cp /usr/share/doc/openvpn/examples/sample-config-files/client.conf /home/user/easy-rsa/keys/
# Accedemos al interior del fichero .conf y descomentamos lo siguiente
nano /etc/vsftpd.conf
# En el interior del fichero de configuracion
#-- Descomentar “write_enable=YES”
#Guardamos y salimos para finalmente reiniciar el servicio vsftpd
service vsftpd restart
```

Conexión por FTP
ftp usuario@ip
contraseña
put ca.crt
put client.conf
put cliente.key
put cliente.crt
put cliente.csr

Rutas e informacion de ficheros de configuracion
```bash
# Información server.conf (servidor)
nano /home/user/easy-rsa/keys/server.conf
# Establecer dirección de los archivos en máquina servidor
ca /home/user/easy-rsa/keys/ca.crt
# Establecer dirección de los archivos en máquina servidor
cert /home/user/easy-rsa/keys/servidor.crt
# Establecer dirección de los archivos en máquina servidor
key /home/user/easy-rsa/keys/servidor.key
# Almacenamiento de IPs que otorgar a los clientes
ifconfig-pool-persist /home/user/easy-rsa/keys/ipp.txt

tls-auth ta.key 0						>>Comentar esta linea
status /home/user/easy-rsa/keys/openvpn-status.log	>>Registro de personas que se loguean

openvpn server.conf						>>Iniciar VPN en el servidor

Información client.conf (cliente)
ca /home/user/ca.crt					>>Establecer dirección de los archivos en máquina cliente
cert /home/user/cliente.crt				s>>Establecer dirección de los archivos en máquina cliente
key /home/user/cliente.key				>>Establecer dirección de los archivos en máquina cliente

tls-auth ta.key 0						>>Comentar esta linea
openvpn user.conf						>>Iniciar VPN en el cliente
```