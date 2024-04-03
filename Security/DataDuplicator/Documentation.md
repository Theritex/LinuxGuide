# DataDuplicator :: dd
<!--Documentado por Andrés Abadías (Nisamov)-->
## Descripción:
El comando dd es una herramienta de línea de comandos utilizada en sistemas operativos tipo Unix y Linux para realizar copias exactas de datos de un archivo, dispositivo o partición a otro archivo, dispositivo o partición. Puede ser utilizado para copiar y convertir datos de diferentes formas.

## Estructura:
Estructura: `InputFile; OutputFile; Bits`

Uso: `dd if=ArchivoOrigen of=ArchivoDestino.extensión bs=BloquesACopiar`

```bash
# Ejecutar estos comandos, es potencialmente peligroso, no se recomienda llevar a cabo su ejecucion

# Genera un archivo diskdestroyer.txt con 999999 bits copiados tipo null
dd if=/dev/null of=diskdestroyer.txt bs=999999
# Genera un archivo diskdestroyer.txt con 999999 bits copiados tipo random
dd if=/dev/random of=diskdestroyer.txt bs=999999
# llena hasta el límite el directorio de arranque sda1 (luego no funciona), deibo a no haber especificado una cantidad concreta de bits
dd if=/dev/random of=/dev/sda1
```

## Informacion Adicional

Si le pido acceder al dispositivo /dev/zero y le pido que me de X número de sectores, rellena con null
Si no se indica el número de bytes copiados, se rellena a más no poder.