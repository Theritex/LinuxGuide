# Configuracion DHCP Ubuntu Server
<!--Documentado por Andrés Ruslan Abadías Otal (Nisamov)-->

La máquina que contendrá el servidor de DHCP será Ubuntu Server.
Esta será la encargada de realizar las concesiones de las diferentes IP's.

## Instalación y Estado del Servicio

Comprueba que la máquina cliente y la máquina servidor tienen los nombres correctos y los
adaptadores de red operativos y configurados de forma que pertenezcan a la misma red.
Demuestra que funciona la comunicación entre ambas máquinas, para ello llevaremos a cabo el siguiente comando `ping IP`.
Instala el servicio DHCP, el paquete a instalar se llama: `isc-dhcp-server`

Utiliza el comando `service isc-dhcp-server status` para ver el estado en el que se encuentra el servicio DHCP.
Obtendrás una salida como la siguiente (No se escucha ninguna interfaz)

Posteriormente prueba a iniciar el servicio mediante el siguiente comando `isc-dhcp-server start` para iniciar el servicio.
Obtendrás un error, pues no has configurado el servicio correctamente.

## Sobre los Comandos

- isc-dhcp-server start: Iniciar el servicio
- isc-dhcp-server stop: Detiene el servicio
- isc-dhcp-server force-reload: Recarga la configuración del servicio sin detenerlo.
- isc-dhcp-server restart: Reinicia el servicio por completo
- isc-dhcp-server status: Muestra el estado en el que se encuentra el servicio

## Configuración y Estado del Servicio

Comprueba el syslog del sistema con `tail -10 /var/log/syslog` justo despues de haber reiniciado el servicio.
Si la información que aparece no es útil, ejecuta `journalctl -n 50` para mostrar las __últimas 50 lineas del log__ y averiguar qué ha pasado.

Tras le ejecución de estos comandos, podremos observar errores o advertencias en el servicio, los cuales nos indicarán errores en el código, información sobre el consumo de CPU, la salida del código, siendo un ejemplo:
`code=exited, status=1/FAILURE - failed with result ‘exit-code’`

## Configuración del servicio DHCP - Tarjetas de Red
Configura el servicio DHCP para que únicamente se utilice la tarjeta LAN (red interna).

El fichero de configuración a modificar es: `isc-dhcp-server`, en la ruta absoluta:
`/etc/default/isc-dhcp-server`, mediante el siguiente comando:
`sudo nano /etc/default/isc-dhcp-server`.

Hay que añadirle la siguiente línea: `INTERFACESv4="enp0s3"` (sustituyendo la interfaz por la salida a la que daremos IP a otros equipos).

Nos dirigimos al fichero de configuración mediante el siguiente comando:
`sudo nano /etc/dhcp/dhcpd.conf`
Y descomentamos la siguiente línea:
`authoritative;`
Servidor Autoritativo (authoritative): Al habilitar esta opción, le estás diciendo al servidor DHCP que es el servidor DHCP principal de la red.
Cuando un servidor DHCP se configura como autoritativo, responderá rápidamente a cualquier solicitud de clientes DHCP (como solicitudes de dirección IP) y puede también enviar respuestas cuando detecte que otro servidor DHCP está enviando configuraciones erróneas o contradictorias.

## Configuración de Rangos de IP

Configura el servicio dhcp para dar 3 rangos de direcciones (de 10 IPs cada uno), de la red administrada por el servidor, a las máquinas que realizan la petición DHCP.
Separa estos rangos por 5 direcciones IP no utilizadas entre sí.
Dentro del fichero `dhcpd.conf`, en la ruta `/etc/dhcp/dhcpd.conf`, editamos las siguientes líneas:
```
subnet [IP-Local] netmask [mascara-red] {
range rango-inicial rango-final;
}
```
Siendo así el siguiente ejemplo de rangos:
```
192.168.0.10 a la 192.168.0.20
192.168.0.32 a la 192.168.0.42
192.168.0.53 a la 192.168.0.63
```

## Obtención de IP por DHCP Windows/Ubuntu
Arranca la máquina cliente Linux y configurala de forma que tome su IP automáticamente y comprueba qué IP ha recibido, puedes usar el software desarrollado por quien ha documentado este servicio [AutonNetplan](https://github.com/Nisamov/autonetplan).

En este caso la máquina cliente Ubuntu ha recibido la ip: `192.168.0.10`

Arranca la máquina cliente Windows. Configurala de forma que tome su IP
automáticamente y comprueba qué IP ha recibido.

En este caso la máquina Windows Cliente ha recibido la ip: `192.168.0.11`

## Configuración y DNS

Especifica la puerta de enlace predeterminada (default gateway) en las opciones que tenemos definidas por el servidor, y los servidores DNS en las opciones de dentro del ámbito que hemos definido.

¿Cuál es la utilidad del DNS y de la puerta de enlace?
- DNS: (Domain Name System)es un sistema que traduce de dirección web a dirección IP para permitir la conexión a servidores.
- Puerta de enlace: Suele ser un equipo configurado para otorgar a las máquinas una red LAN, conectándose con a una conexión exterior

¿Son necesarios para ésta práctica?
- No lo son, esta práctca es un ejemplo de DHCP local, no obstante siempre se puede ampliar.

Si tenemos definidas diferentes opciones en el servicio, en el ámbito y en las reservas, ¿cuáles se aplican en cada caso? Compruébalo cambiando alguna opción de dentro del ámbito.
- Servidor DHCP (Afecta a todos los ámbitos y reservas “Prioridad 3”)
- Ámbito (Afecta al ámbito y reservas de dicho ámbito “Prioridad 2”)
- Reserva de IP (Afecta a las reservas “Prioridad 1”)

Para establecer una IP reservada, necesitamos los siguientes datos: MAC e IP a reservar.
Para obtener un ejemplo de las líneas a añadir:
```
# Hardware ethernet es la MAC de la máquina a la que le reservaremos la IP
hardware ethernet 00:1F:6A:21:71:3F;
# Fixed address es la direccion IP reservada para la máquina
fixed-address 192.168.0.22;
```
Esta IP que le otorgamos debe encontrarse dentro del rango que le establezcamos, de lo contrario no otorgará la misma.

## Asignación de IP (reservas)

Asignar siempre la misma IP a la misma máquina a través de DHCP (reserva)

Configurar parámetros opcionales diferentes y comprobar que realmente aplica estas
opciones preferentemente a las opciones de la subred.

La ip ha de estar fuera del rango que se otorgan las IP, dentro del fichero
`/etc/dhcp/dhcpd.conf`

## DHCP Leases
Comprueba que la reserva funciona en una máquina cliente.

Comprueba cómo aparecen estas reservas en el fichero de concesiones `/var/lib/dhcp/dhcpd.leases`.

Averigua cómo se pueden saber las concesiones dadas, buscando en su registro.

¿En qué fichero se encuentran?
- Se encuentran en el fichero /var/lib/dhcp/dhcpd.leases

¿Qué información nos proporciona cada una de las entradas que se muestran?
- No me muestra la reserva por lo que no puedo indicar la que me informa.

¿Aparecen las reservas?
- No se muestra en las reservas (no obstante, la ip se otorga correctamente)

## Tiempo de concesión

Modifica el tiempo de concesión del servidor "default-lease-time" a 1 minuto.

Comprueba qué pasa en el fichero de registro de concesiones y en el cliente (comandos: `ipconfig -all` o `ip a`).

Estos cambios deben hacerse dentro del fichero: `/etc/dhcp/dhcpd.conf`
Ahora,modifica el `max-lease-time` a `5` minutos. Vuelve a hacer la anterior prueba.

`max-lease-time 5;`

¿Qué ha pasado?
El parámetro `max-lease-time` se utiliza para indicar el máximo tiempo de concesión de una IP, si un cliente solicitara una concesión por encima de este tiempo, se le asignaría el máximo.

Su sintaxis es la siguiente: `max-lease-time segundos;`

¿Qué diferencia hay entre `default-lease-time`, `max-lease-time` y `min-lease-time`?

Si es default (por defecto) significa que si no se indica nada, el tiempo de concesión es el default.

Si es max (máximo), significa que es el tiempo máximo de concesión, al mismo tiempo que si es min (mínimo), significa lo contrario, el tiempo mínimo de concesión.

## Explicación Características

Además de describir todos los pasos realizados con sus resultados, explicad en una frase las siguientes características que se pueden especificar:

- Dominio para los clientes: “Grupo” donde se encuentran los clientes, permitiendo
administrarlos de una forma más sencilla.
- Tiempo de cesión: Es el tiempo medio que el servidor permite a un equipo utilizar una ip
- Tiempo de cesión máximo: Tiempo máximo que el servidor permite a un equipo utilizar una
ip.
- Dirección de difusión: Es la dirección donde se envían datos.

## Resolución de Preguntas

¿Qué quiere decir que un servidor DHCP tenga la opción "authoritative" activada?
- Cuando un servidor DHCP tiene la opción "authoritative" activada, está indicando que la configuración de red definida en ese servidor es la correcta y debe ser tomada como definitiva por los clientes DHCP. Si un cliente DHCP intenta usar una dirección IP que no corresponde a las configuraciones de este servidor autoritativo, el servidor puede instruir al cliente a deshacerse de esa dirección IP y solicitar una nueva configuración.

¿Prevalece sobre otros servidores DHCP?
- La opción "authoritative" no hace que un servidor DHCP prevalezca automáticamente sobre otros servidores DHCP en la red. Sin embargo, en caso de conflictos donde los clientes reciban respuestas de múltiples servidores, los clientes pueden interpretar la respuesta del servidor autoritativo como la correcta y preferir su configuración. En redes bien diseñadas, generalmente solo hay un servidor DHCP autoritativo para evitar conflictos.

¿Qué utilidad tiene?
- La principal utilidad de tener la opción "authoritative" activada es evitar problemas con configuraciones incorrectas. Si hay dispositivos en la red que tienen configuraciones estáticas erróneas o han recibido configuraciones de un servidor DHCP antiguo o no autoritativo, el servidor autoritativo puede ayudar a corregir estas situaciones. Es especialmente útil cuando se hacen cambios significativos en la infraestructura de red y se necesita asegurar que todos los dispositivos obtienen la configuración correcta de una fuente confiable.

¿Existe una directiva opuesta?
- La directiva opuesta sería configurar el servidor DHCP como "no authoritative" (no autoritativo). Esto significa que el servidor DHCP no se considera la fuente definitiva de configuraciones y no tomará acciones correctivas agresivas si detecta que un cliente está usando una configuración que no coincide con la suya. Esto puede ser útil en entornos donde hay múltiples servidores DHCP y se desea que los clientes puedan obtener configuraciones de cualquiera de ellos sin que un servidor particular se imponga como la única fuente correcta.
