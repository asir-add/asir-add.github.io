---
title: Proyecto Check-Files
---

Este proyecto consiste en implementar un script para el PowerShell de Windows que verifique si existen o no y si han cambiado una serie de archivos y subcarpetas de una carpeta dada. Esta información se leerá de un archivo como el siguiente:

```powershell
c:\micarpeta						<-- primera línea, la carpeta contenedora
fichero1.txt,11/03/2013 13:35:00	<-- resto de líneas, los archivos y subcarpetas a comprobar
fichero2.pdf,10/01/2012 11:17:22
subcarpeta1,30/11/2012 12:21:08
documento1.docx,23/04/2010 23:07:18
documento2.odt,08/01/2013 05:49:39
```

La primera línea es la carpeta que contiene a los archivos y subcarpetas, y el resto de líneas son los archivos y subcarpetas a comprobar. El script comprobará para cada archivo y subcarpeta, si existe o no, y en caso de existir, si la fecha de modificación del mismo ha cambiado o no.

El script deberá mostrar un listado de los cambios producidos.

* El nombre del script será `Check-Files.ps1`.

## Sintaxis

* La sintaxis del script es la siguiente:

```powershell
Check-Files.ps1 [ --help | --check fichero | --generate fichero --target carpeta ]
```

* El funcionamiento del script será el siguiente:

| Opción                                | Descripción                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| `--help`                              | Muestra la ayuda de sí mismo: `Get-Help .\Check-Files.ps1`   |
| `--check fichero `                    | Comprueba si ha habido cambios en la carpeta usando el `fichero` de control pasado por parámetro. Para cada cambios mostrar información del cambio: el fichero ha sido modificado (por su fecha/hora), el fichero ha sido creado o el fichero ya no existe. |
| `--generate fichero --target carpeta` | Para la `carpeta` indicada genera el `fichero` de control.   |

> El script deberá contener los comentarios de ayuda de PowerShell, de forma que se muestre toda la información del mismo con `Get-Help`.

### Ejemplos de uso

Suponiendo que hay un pendrive en la unidad `E:` conteniendo el fichero `autorun.inf` anterior y otro en la unidad `F:` sin éste fichero.

```powershell
# Comprobar si hay virus sólo en la unidad E:
PS> .\Clean-MemoryStick.ps1 --check E:
E: - peazovirus.exe

# Comprobar si hay virus en todas las unidades (E: y F: en este caso)
PS> .\Clean-MemoryStick.ps1 --check
E: - peazovirus.exe
F: - No se ha encontrado nada.

# Eliminar virus de la unidad E:
PS> .\Clean-MemoryStick.ps1 --clean E:
E: - peazovirus.exe
¿Desea eliminar el ejecutable? (S/N): s
El ejecutable peazovirus.exe ha sido eliminado de la unidad E:

# Poner en cuarentena virus en la unidad E:
PS> .\Clean-MemoryStick.ps1 --clean E: --quarantine
E: - peazovirus.exe
¿Desea poner el ejecutable en cuarentena? (S/N): s
El ejecutable peazovirus.exe de la unidad E: ha sido puesto en cuarentena.
```

## Calificación

| Opción               | Funcionalidad                                       | Peso (%) |
| -------------------- | --------------------------------------------------- | :------: |
| --help               | Mostrar la ayuda.                                   |    15    |
| --check              | Buscar ejecutables en `autorun.inf`.                |    30    |
| --clean              | Eliminar ejecutables en `autorun.inf`.              |    40    |
| --clean --quarantine | Mover ejecutables en `autorun.inf` a `.quarantine`. |    15    |