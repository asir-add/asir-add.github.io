---
title: Proyecto Clean-MemoryStick
---

Es habitual que cuando trabajamos con pendrives y vamos de un equipo para otro el pendrive acabe infectado con algún tipo de virus. Estos virus se aprovechan de una característica de Windows: el **autoarranque** (autorun). El autoarranque consiste en que si en la raíz del pendrive existe el fichero `autorun.inf`, Windows lo analiza y ejecuta lo que indique ese fichero (pudiendo ser un virus oculto en el propio pendrive).

El contenido del fichero puede ser el siguiente:

```ini
[autorun]
icon=machanguito.ico
; esto es un comentario
label=MiPendrive
shellexecute=http://www.informaticofriki.blogspot.com/
open=peazovirus.exe
...
```

En concreto la línea con la opción  `open` es la que indica el ejecutable del pendrive que se va a
iniciar (y que para nosotros es sospechoso).

Este proyecto consiste en implementar un script para el PowerShell de Windows que permita chequear y limpiar pendrives de este tipo de amenazas.

* El nombre del script será `Clean-MemoryStick.ps1`.

## Sintaxis

* La sintaxis del script es la siguiente:

```powershell
Clean-MemoryStick.ps1 [ --help | --check [unidad] | --clean [unidad] [--quarantine] ]
```

* El funcionamiento del script será el siguiente:

| Opción                            | Descripción                                                  |
| --------------------------------- | ------------------------------------------------------------ |
| `--help`                          | Muestra la ayuda de sí mismo: `Get-Help .\Clean-MemoryStick.ps1` |
| `--check [unidad] `               | Busca en todos los dispositivos removibles (pendrives) si existe fichero `autorun.inf`; si existe, se extraerá el nombre del ejecutable de la opción `open`  y se mostrará la letra de la unidad y el nombre del ejecutable.  Si se especifica la letra de la unidad (D:, E:, F:, ...), sólo se comprobará esa unidad. |
| `--clean [unidad] [--quarantine]` | Busca en todos los dispositivos removibles (pendrives) si existe fichero `autorun.inf`; si existe, se extraerá el nombre del ejecutable de la opción `open`  y se preguntará al usuario si se quiere eliminar el ejecutable de la unidad correspondiente.  Si se especifica la letra de la unidad (D:, E:, F:, ...), sólo se comprobará esa unidad. Si se especifica además la opción `--quarantine`, en vez de eliminar los archivos ejecutables, se moverán a la carpeta `.quarantine` en el directorio HOME del usuario (`$HOME\.quarantine`). Este directorio deberá crearse si no existe. |

> El script deberá contener los comentarios de ayuda de PowerShell, de forma que se muestre toda la información del mismo con `Get-Help`.

### Ejemplos de uso

Suponiendo que hay un pendrive en la unidad `E:` conteniendo el fichero `autorun.inf` anterior y otro en la unidad `F:` sin éste fichero.

```powershell
# Muestra la ayuda del script
PS> .\Clean-MemoryStick.ps1 --help
# ó
PS> Get-Help .\Clean-MemoryStick.ps1

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