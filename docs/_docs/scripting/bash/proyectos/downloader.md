---
title: Proyecto DOWNLOADER
---

## Descripción

Muchas veces cuando descargamos algo de Internet ocurre que ya lo hemos descargado antes y nos gustaría saber cuando ocurre eso.

Este proyecto consiste en elaborar un script para la BASH en GNU/Linux que nos ayude a controlar nuestras descargas:

* El nombre del script será `downloader.sh`.
* Los ficheros descargados se guardarán en un directorio en nuestro directorio de usuario (HOME). Es decir, si nosotros somos el usuario **chanquete**, nuestro directorio de descargas será `/home/chanquete/descargas`. 
* El directorio HOME del usuario que ejecuta el script lo podemos sacar de la variable `$HOME`. Si no existe el directorio `descargas` se deberá crear.


* En el directorio de `descargas` se creará el fichero oculto `.registro` donde se añadirá al final cada URL descargada.

## El comando `wget`

* Para descargar un fichero desde una URL se usará el comando `wget`.
* Por ejemplo:

```BASH
wget --tries=5 --directory-prefix=/directorio/destino –q <url>
```

> Un ejemplo de URL sería: [http://downloads.sourceforge.net/sevenzip/7z920.tar.bz2](http://downloads.sourceforge.net/sevenzip/7z920.tar.bz2), de forma que el fichero descargado se llamaría `7z920.tar.bz2`.

```bash
wget --tries=5 --directory-prefix=/home/chanquete –q http://dow....et/sevenzip/7z920.tar.bz2
```

* Las opciones utilizadas con el comando `wget` son:

| Opción                       | Descripción                              |
| ---------------------------- | ---------------------------------------- |
| `--tries=X`                  | Indica el número de reintentos en caso de que falle la descarga (`X` es el número de reintentos). |
| `--directory-prefix=destino` | Indica el directorio donde se va a guardar el fichero descargado. |
| `-q`                         | Indica al comando que no muestre ningún tipo de información en consola. |

## Sintaxis

* La sintaxis del script es la siguiente:

```bash
downloader.sh [ --help | --estadisticas | --limpiar | --registro | url ]
```

* El funcionamiento del script será el siguiente:

| Opción           | Descripción                              |
| ---------------- | ---------------------------------------- |
| Sin opciones     | Se mostrará la ayuda (igual que `--help`). |
| `--help`         | Muestra la ayuda, explicando para qué sirve el script, sus distintas opciones y cómo se utiliza. |
| `--estadisticas` | Muestra **la cantidad de bytes descargados** (sumar los bytes de todos los ficheros que hay en `descargas`), **la cantidad de ficheros descargados** (número de ficheros que hay en `descargas`), y **el promedio de bytes por fichero** (bytes de todos los ficheros entre el número de ficheros). |
| `--limpiar`      | Elimina todo el contenido del directorio de descargas de forma definitiva, así como elfichero oculto “.registro”. Se mostrará un mensaje pidiendo confirmación; si el usuario responde “si”,se vaciará, de lo contrario no se hará nada. |
| `--registro`     | Muestra el contenido del fichero “.registro”, si existe, con el listado de todas las URLsdescargadas. |
| `url`            | Dirección URL con el fichero que queremos descargar. Si el directorio `descargas` no existe, sedeberá crear. Una vez descargado el fichero dentro de `descargas`, se deberá registrar la URL en el fichero `.registro`. Antes de hacer la descarga se deberá comprobar que no se ha descargado ya la URL indicada por el usuario; de ser así, se cancela la descarga indicando el motivo: "*La URL xxx ya ha sido descargada*". Si hay error en la descarga (consultar la variable `$?`) no se registra. |

> Se tendrá en cuenta que se verifiquen los parámetros especificados por el usuario y se controlen, en la medida de lo posible, los errores. Además, el script deberá poder ser utilizado por cualquier usuario del sistema sin necesidad de cambiar el código del mismo.

## Calificación

| Apartado          | Funcionalidad                            | Peso (%) |
| ----------------- | ---------------------------------------- | :------: |
| Sin opciones      | Muestra la ayuda del comando.            |    5     |
| `--help`          | Igual al apartado anterior.              |    10    |
| `--estadisticas`  | Muestra las estadísticas.                |    20    |
| `--limpiar`       | Elimina el contenido del directorio `descargas`. |    10    |
| `--registro`      | Muestra todas las URLs descargadas y registradas en `.registro`. |    10    |
| `url`             | Descarga el fichero en el directorio `descargas`. |    25    |
| `url`             | Se comprueba si ya se ha descargado antes consultando el fichero `.registro`. |    10    |
| Cualquier usuario | El script está preparado para ser usado por cualquier usuario, sin necesidad demodificarlo. |    10    |