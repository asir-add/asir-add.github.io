---
title: Proyecto REDMIN
---

## Descripción

Muchas veces, desde la Shell de GNU/Linux, necesitamos cambiar la configuración de las interfaces de red de nuestro ordenador, lo cual es una tarea un poco tediosa y que requiere recordar los comandos que hay que usar así comosus distintas opciones.

Este proyecto consiste en elaborar un script para la BASH en GNU/Linux que nos facilite la configuración de las interfaces de red sirviendo como asistente de configuración de las interfaces de red ("redmin" proviene de la combinación de **RED** y ad**MIN**istrador):

* El nombre del script será `redmin.sh`.

## Sintaxis

* La sintaxis del script es la siguiente:

```bash
redmin.sh [ --help | --listar | --consultar interfaz | --manual interfaz | --auto interfaz ]
```

* El funcionamiento del script será el siguiente:

| Opción                 | Descripción                              |
| ---------------------- | ---------------------------------------- |
| `interfaz`             | Es el nombre de la interfaz de red (por ejemplo: `lo`, `eth0`, `wlan1`, ...). |
| `--help`               | Muestra la ayuda, explicando para qué sirve el script, sus distintas opciones y cómo se utiliza. |
| `--listar`             | Muestra los nombres de todas las interfaces de red disponibles en el equipo (por ejemplo: `lo`, `eth0`, `wlan1`, ...). |
| `--consultar interfaz` | Muestra la configuración actual de la interfaz de red especifica, esto es,dirección IP, máscara de red, servidor DNS y puerta de enlace. |
| `--manual interfaz`    | Configura la interfaz especificada de forma manual; esto es, se le pedirá al usuarioque introduzca desde teclado la dirección IP, la máscara de red, la puerta de enlace predeterminada yel servidor DNS. |
| `--auto interfaz`      | Configura la interfaz de red de forma automática (mediante DHCP); lo único que sepedirá que introduzca el usuario es el servidor DNS y la puerta de enlace predeterminada. |
| Sin opciones           | Se mostrará un menú que permitirá seleccionar la interfaz de red que se quiereconfigurar de entre las disponibles, y luego se pedirá si quiere configurarse de forma manual oautomática. Se pueden reutilizar las opciones del script `--listar`, `--manual` y `--auto`. |

> Consultar los comandos `ifconfig`, `dhclient` y `route`, y el fichero de configuración `/etc/resolv.conf`.

> Se tendrá en cuenta que se verifiquen los parámetros especificados por el usuario y se controlen, en la medida de lo posible, los errores.

>  El script se tendrá que ejecutar como `root` para poder cambiar la configuración de red.

## Calificación

| Opción       | Funcionalidad                            | Peso (%) |
| ------------ | ---------------------------------------- | :------: |
| Sin opciones | Mostrar un menú que permita elegir interfaz y forma de configuración. |    20    |
| --help       | Mostrar la ayuda del comando.            |    10    |
| --listar     | Listar las interfaces de red disponibles. |    15    |
| --consultar  | Mostrar la configuración actual de una interfaz. |    15    |
| --manual     | Configuración manual de una interfaz.    |    20    |
| --auto       | Configuración automática de una interfaz. |    20    |