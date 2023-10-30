---
title: Proyecto Compressor
---

Este proyecto consiste en implementar un script para el PowerShell de Windows que facilite el uso del compresor [7-Zip](http://www.7-zip.org/) desde la línea de comandos.

* El nombre del script será `Compressor.ps1`.

## Sintaxis

La sintaxis del script es la siguiente:

```powershell
Compressor.ps1 [ -Show fichero  | -Add fichero | -Remove fichero | [ -Extract fichero | -ExtractAll ] -Target carpeta ] -Path comprimido
```

El funcionamiento del script será el siguiente:

| Opción        | Descripción                                                                                                                                                                                                                                                                                                   |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sin opciones  | El comando entra en modo interactivo, mostrando inicialmente un menú con las distintas opciones del comando.                                                                                                                                                                                                  |
| `-Show `      | Abre el `fichero` dentro del `comprimido`. Se deberá extraer el fichero al directorio temporal, abrirlo con el programa adecuado (por ejemplo, si es un TXT se abrirá con el Bloc de notas, si es un PDF se abrirá con el Adobe Reader, ...). Al final se deberá eliminar el fichero del directorio temporal. |
| `-Add`        | Añadir el `fichero` al `comprimido`. Si `comprimido` no existe, deberá crearse.                                                                                                                                                                                                                               |
| `-Remove`     | Eliminar el `fichero` del `comprimido`.                                                                                                                                                                                                                                                                       |
| `-Extract`    | Extraer el `fichero` del `comprimido` a la `carpeta` indicada.                                                                                                                                                                                                                                                |
| `-ExtractAll` | Extraer todos los ficheros del `comprimido` a la `carpeta` indicada.                                                                                                                                                                                                                                          |
| `-Path`       | Ruta del archivo comprimido con el que se quiere trabajar.                                                                                                                                                                                                                                                    |

> El script deberá contener los comentarios de ayuda de PowerShell, de forma que se muestre toda la información del mismo con `Get-Help`.

### Ejemplos de uso

Suponiendo que hay un pendrive en la unidad `E:` conteniendo el fichero `autorun.inf` anterior y otro en la unidad `F:` sin éste fichero.

```powershell
# Muestra la ayuda del script
PS> Get-Help .\Compressor.ps1

# Modo interactivo
PS> .\Compressor.ps1 -Path ejemplo.7z
Compressor
----------
1) Mostrar fichero
2) Añadir fichero
3) Eliminar fichero
4) Extraer fichero
5) Extraer todos
Elija una opción: 1<ENTER>

1. Mostrar fichero
Indique el nombre del fichero a mostrar: prueba.txt
[...]

# Mostrar un fichero del comprimido
PS> .\Compressor.ps1 -Show prueba.txt -Path ejemplo.7z
<Se abre prueba.txt con la aplicación asociada a los ficheros TXT>

# Añadir un fichero al comprimido
PS> .\Compressor.ps1 -Add prueba.txt -Path ejemplo.7z
Se ha añadido el fichero prueba.txt a ejemplo.7z.

# Eliminar un fichero del comprimido
PS> .\Compressor.ps1 -Remove prueba.txt -Path ejemplo.7z
Se ha eliminado el fichero prueba.txt de ejemplo.7z.

# Extraer un fichero del comprimido
PS> .\Compressor.ps1 -Extract prueba.txt -Target c:\micarpeta -Path ejemplo.7z
Se ha extraído el fichero prueba.txt de ejemplo.7z al directorio c:\micarpeta.

# Extraer todo del comprimido
PS> .\Compressor.ps1 -ExtractAll -Target c:\micarpeta -Path ejemplo.7z
Se ha extraído todo el contenido de ejemplo.7z al directorio c:\micarpeta.
```

### Pistas

Para ejecutar el 7-Zip desde línea de comandos:

```powershell
PS> & "C:\Program Files\7-Zip\7z" "argumento1" "argumento2" ... | Out-Null
```

> El operador `&` indica que la siguiente cadena de caracteres debe ejecutarse, y esto es necesario porque  la ruta al ejecutable `7z.exe` tiene espacios en blanco.

> El cmdlet `Out-Null` elimina la salida producida por el comando, para que no salga en la consola.

Añadir un fichero a un comprimido (si no existe el comprimido, se crea):

```powershell
PS> & "C:\Program Files\7-Zip\7z" "a" "-y" "comprimido" "fichero" | Out-Null
```

Elimina un fichero de un comprimido:

```powershell
PS> & "C:\Program Files\7-Zip\7z" "d" "comprimido" "fichero" | Out-Null
```

Extrae un fichero de un comprimido a una carpeta de destino:

```powershell
PS> & "C:\Program Files\7-Zip\7z" "e" "-y" "-odestino" "comprimido" "fichero" | Out-Null
```

Extrae todos el contenido de un comprimido a una carpeta de destino:

```powershell
PS> & "C:\Program Files\7-Zip\7z" "e" "-y" "-odestino" "comprimido" | Out-Null
```

Ruta del directorio temporal del usuario:

```powershell
PS> $env:TEMP
```

Abrir un fichero con el programa asociado:

```powershell
PS> Start-Process -FilePath fichero
```

> Por ejemplo, para abrir un PDF con el Adobe Reader o el programa asociado a los ficheros con extensión `.PDF`: 
> 
> ` Start-Process -FilePath ".\documento.pdf"`

## Calificación

| Opción | Funcionalidad     | Peso (%) |
| ------ | ----------------- |:--------:|
| -Help  | Mostrar la ayuda. | 10       |
|        |                   |          |
|        |                   |          |
|        |                   |          |