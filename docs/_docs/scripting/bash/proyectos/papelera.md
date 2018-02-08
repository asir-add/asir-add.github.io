---
title: Proyecto Papelera
---

## Descripción

En la interfaz gráfica (GNOME, KDE, etc.) disponemos de una papelera de reciclaje, de forma que cuando borramos algo, en lugar de eliminarlo, se envía a esta papelera. Así, si por error hemos eliminado algo, siempre podremos recuperarlo; a no ser que hayamos vaciado la papelera. Sin embargo, cuando utilizamos el comando `rm` en GNU/Linux, ya sea para eliminar directorios o ficheros, éstos son eliminados de forma permanente.

Este proyecto consiste en elaborar un script para la BASH que nos permitan disponer de una papelera de reciclaje en un terminal de texto:

* El nombre del script será `borrar.sh`.
* La papelera de reciclaje será un directorio oculto en nuestro directorio de usuario (`HOME`). Es decir, si nosotros somos el usuario **chanquete**, nuestra papelera será `/home/chanquete/.papelera` (recuerda que los ficheros y directorios ocultos empiezan por `.`). El directorio `HOME` del usuario que ejecuta el script lo podemos sacar de la variable `$HOME`.

## Sintaxis del script

* La sintaxis del script es la siguiente:

```bash
borrar.sh [ --help | -r fichero [ destino ] | --estadisticas | --vaciar | fichero ]
```

* El funcionamiento del script será el siguiente:

| Opción               | Descripción                              |
| -------------------- | ---------------------------------------- |
| Sin opciones         | Se mostrará un menú que permitirá seleccionar entre vaciar la papelera o mostrar las estadísticas. |
| `--help`             | Muestra la ayuda, explicando para qué sirve el script, sus distintas opciones y cómo se utiliza. |
| `-r fichero destino` | Recupera un fichero o directorio, es decir, mueve el `fichero` (o directorio) de la papelera al directorio de `destino`. Si no se especifica el destino, se deberá utilizar el directorio actual. |
| `--estadisticas`     | Se mostrará un pequeño informe indicando el número de ficheros que hay en la papelera y el número de directorios (de forma separada). Tener en cuenta los ocultos y no contar los directorios `.` y `..`. |
| `--vaciar`           | Elimina todo el contenido de la papelera de forma definitiva. Se mostrará un mensaje pidiendo confirmación para vaciar la papelera, a no ser que la papelera no exista o ya esté vacía. Si el usuario responde "**si**", se vaciará, de lo contrario no se hará nada. |
| `fichero`            | Nombre del fichero o directorio que queremos enviar a la papelera. Si el directorio ".papelera" no existe, se deberá crear. |

> Se verificarán los parámetros especificados por el usuario y se controlará, en la medida de lo posible, los errores. Además, el script deberá poder ser utilizado por cualquier usuario del sistema, cada uno con su papelera.

## Calificación

| Apartado          | Funcionalidad                            | Peso (%) |
| ----------------- | ---------------------------------------- | :------: |
| Sin opciones      | Mostrar un menú que permita elegir entre vaciar la papelera o mostrar las estadísticas. |    15    |
| --help            | Muestra la ayuda del comando.            |    10    |
| -r                | Restaura un fichero o directorio eliminado. |    20    |
| --estadisticas    | Muestra las estadísticas.                |    15    |
| --vaciar          | Elimina el contenido de la papelera.     |    10    |
| fichero           | Envía un fichero a la papelera.          |    20    |
| Cualquier usuario | El script puede ser utilizado por cualquier usuario (cada uno con su propia papelera) sin necesidad de modificarlo. |    10    |

