---
title: Proyecto Check-Files
---

Este proyecto consiste en implementar un script para el PowerShell de Windows que verifique si existen o no y si han cambiado una serie de archivos y subcarpetas de una carpeta dada. Esta información se leerá de un archivo como el siguiente:

```
C:\micarpeta                      <-- primera línea, la carpeta contenedora
fichero1.txt,11/03/2013 13:35:00  <-- resto de líneas, los archivos y subcarpetas a comprobar
fichero2.pdf,10/01/2012 11:17:22
subcarpeta1,30/11/2012 12:21:08
documento1.docx,23/04/2010 23:07:18
documento2.odt,08/01/2013 05:49:39
```

La primera línea es la carpeta que contiene a los archivos y subcarpetas, y el resto de líneas son los archivos y subcarpetas a comprobar. El script comprobará para cada archivo y subcarpeta, si existe o no, y en caso de existir, si la fecha de modificación del mismo ha cambiado o no.

El script deberá mostrar un listado de los cambios producidos.

* El nombre del script será `Check-Files.ps1`.

## Sintaxis

La sintaxis del script es la siguiente:

```powershell
Check-Files.ps1 [ -Help | -Check fichero | -Generate fichero -Target carpeta ]
```

El funcionamiento del script será el siguiente:

| Opción                              | Descripción                                                  |
| ----------------------------------- | ------------------------------------------------------------ |
| `-Help`                             | Muestra la ayuda de sí mismo: `Get-Help .\Check-Files.ps1`   |
| `-Check fichero `                   | Comprueba si ha habido cambios en la carpeta usando el `fichero` de control pasado por parámetro. Los cambios a controlar y notificar son los siguiente: el fichero ha sido modificado (por su fecha/hora), el fichero ha sido creado o el fichero ya no existe. |
| `-Generate fichero -Target carpeta` | Para la `carpeta` indicada genera el `fichero` de control.   |

> El script deberá contener los comentarios de ayuda de PowerShell, de forma que se muestre toda la información del mismo con `Get-Help`.

### Ejemplos de uso

Suponiendo que disponemos de la carpeta `C:\micarpeta`.

```powershell
PS C:\micarpeta> Get-ChildItem

    Directorio: C:\micarpeta

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       30/11/2012     12:21                subcarpeta1
-a----       23/04/2010     23:07          70011 documento1.docx
-a----       08/01/2013     05:49          34234 documento2.odt
-a----       11/03/2013     13:35           1045 fichero1.txt
-a----       10/01/2012     11:17           2047 fichero2.pdf

# Genera el fichero de control "info.txt" para el directorio "C:\micarpeta"
PS C:\> .\Check-Files.ps1 -Generate info.txt -Target C:\micarpeta
Fichero de control info.txt generado para la carpeta C:\micarpeta

# Muestra el contenido del fichero de control generado
PS C:\> Get-Content info.txt
C:\micarpeta
fichero1.txt,11/03/2013 13:35:00
fichero2.pdf,10/01/2012 11:17:22
subcarpeta1,30/11/2012 12:21:08
documento1.docx,23/04/2010 23:07:18
documento2.odt,08/01/2013 05:49:39

# Comprueba si ha habido cambios
PS C:\> .\Check-Files.ps1 -Check info.txt
Comprobando cambios en C:\micarpeta:
- No ha habido cambios, todo sigue igual

# Aplicamos algunos cambios sobre el contenido del directorio "c:\micarpeta"
PS C:\micarpeta> Remove-Item fichero2.pdf        # Elimina fichero2.pdf
PS C:\micarpeta> "Nueva línea" >> fichero1.txt   # Añade una línea al final de fichero1.txt
PS C:\micarpeta> "Nuevo fichero" > nuevo.txt     # Crea el fichero nuevo.txt

# Comprueba si ha habido cambios
PS C:\> .\Check-Files.ps1 -Check info.txt
Comprobando cambios en C:\micarpeta:
- fichero1.txt ha cambiado
- fichero2.pdf ya no existe
- nuevo.txt ha sido creado
```

## Calificación

| Opción    | Funcionalidad                   | Peso (%) |
| --------- | ------------------------------- | :------: |
| -Help     | Mostrar la ayuda.               |    10    |
| -Check    | Comprueba si ha habido cambios. |    45    |
| -Generate | Genera el fichero de control.   |    45    |