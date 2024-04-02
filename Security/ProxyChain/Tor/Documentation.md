# Proxy Chains - Tor
<!--Documentado por Andrés Abadías (Nisamov)-->
## Requisitos: Instalación de Tor Browser

Descripción:
Proxychains es un software de código abierto para sistemas Linux y viene preinstalado con Kali Linux, la herramienta redirige las conexiones TCP a través de proxies como TOR, SOCKS4, SOCKS5 y HTTP (S) y nos permite encadenar servidores proxy. Con las cadenas de proxy, se puede ocultar la dirección IP del tráfico de origen y evadir IDS y firewalls.
```bash
sudo -i			            >> Acceso como root
service Tor status		    >> Comprobamos si Tor esta instalado siguiendo su estado
apt-get install tor		    >> Instalamos Tor si no está instalado todavía
service tor start		    >> Iniciamos el servicio tor
service Tor status		    >> Comprobamos si Tor está operativo
nano /etc/proxychains.conf	>> Entramos en la configuración de las proxychains
```
## Configuración:
```
Eliminar comentario de cadena dinámica
comentar Cadena estricta y cadena aleatoria
Eliminar comentario de DNS proxy
Eliminar solicitudes de DNS de proxy (no se filtran datos de DNS del comentario)
Escribir calcetines5 127.0.0.1 9050 en la última línea de la lista de proxy
```
Tras aplicar la configuracion, reiniciamos el servicio de tor:
```bash
# Reinicio de servicio Tor
service Tor restart
```
Configuracion ProxyChain - Tor
La configuracion final se puede encontrar en la siguiente linea:
>Linea 164 y 165 en el fichero [proxychain.conf](Security\ProxyChain\Tor\proxychain.conf)