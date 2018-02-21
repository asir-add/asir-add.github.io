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

La sintaxis del script es la siguiente:

```powershell
Clean-MemoryStick.ps1 [ -Help | -Check [unidad] | -Clean [unidad] [-Quarantine] ]
```

El funcionamiento del script será el siguiente:

| Opción                          | Descripción                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| `-Help`                         | Muestra la ayuda de sí mismo: `Get-Help .\Clean-MemoryStick.ps1` |
| `-Check [unidad] `              | Busca en todos los dispositivos removibles (pendrives) si existe fichero `autorun.inf`; si existe, se extraerá el nombre del ejecutable de la opción `open`  y se mostrará la letra de la unidad y el nombre del ejecutable.  Si se especifica la letra de la unidad (D:, E:, F:, ...), sólo se comprobará esa unidad. |
| `-Clean [unidad] [-Quarantine]` | Busca en todos los dispositivos removibles (pendrives) si existe fichero `autorun.inf`; si existe, se extraerá el nombre del ejecutable de la opción `open`  y se preguntará al usuario si se quiere eliminar el ejecutable de la unidad correspondiente.  Si se especifica la letra de la unidad (D:, E:, F:, ...), sólo se comprobará esa unidad. Si se especifica además la opción `-Quarantine`, en vez de eliminar los archivos ejecutables, se moverán a la carpeta `.quarantine` en el directorio HOME del usuario (`$HOME\.quarantine`). Este directorio deberá crearse si no existe. |

> El script deberá contener los comentarios de ayuda de PowerShell, de forma que se muestre toda la información del mismo con `Get-Help`.

### Ejemplos de uso

Suponiendo que hay un pendrive en la unidad `E:` conteniendo el fichero `autorun.inf` anterior y otro en la unidad `F:` sin éste fichero.

```powershell
# Muestra la ayuda del script
PS> .\Clean-MemoryStick.ps1 -Help
# ó
PS> Get-Help .\Clean-MemoryStick.ps1

# Comprobar si hay virus sólo en la unidad E:
PS> .\Clean-MemoryStick.ps1 -Check E:
E: - peazovirus.exe

# Comprobar si hay virus en todas las unidades (E: y F: en este caso)
PS> .\Clean-MemoryStick.ps1 -Check
E: - peazovirus.exe
F: - No se ha encontrado nada.

# Eliminar virus de la unidad E:
PS> .\Clean-MemoryStick.ps1 -Clean E:
E: - peazovirus.exe
¿Desea eliminar el ejecutable? (S/N): s
El ejecutable peazovirus.exe ha sido eliminado de la unidad E:

# Poner en cuarentena virus en la unidad E:
PS> .\Clean-MemoryStick.ps1 -Clean E: -Quarantine
E: - peazovirus.exe
¿Desea poner el ejecutable en cuarentena? (S/N): s
El ejecutable peazovirus.exe de la unidad E: ha sido puesto en cuarentena.
```

### Pistas

Para obtener todas las unidades de disco (Provider.Name == FileSystem) montadas (Free != nulo) en el sistema:

```powershell
PS> Get-PSDrive | Where-Object { $_.Provider.Name -eq "FileSystem" -and $_.Free -ne $null }
```

## Calificación

| Opción             | Funcionalidad                                       | Peso (%) |
| ------------------ | --------------------------------------------------- | :------: |
| -Help              | Mostrar la ayuda.                                   |    10    |
| -Check             | Buscar ejecutables en `autorun.inf`.                |    30    |
| -Clean             | Eliminar ejecutables en `autorun.inf`.              |    45    |
| -Clean -Quarantine | Mover ejecutables en `autorun.inf` a `.quarantine`. |    15    |