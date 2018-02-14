---
title: Scripting en PowerShell
---

## El primer script

* El script "Hola Mundo" (holamundo.ps1):

```powershell
Write-Host "Hola Mundo"
```
* Ejecutar: `.\holamundo.ps1`
  * Hay que añadir “.\” porque “holamundo.ps1” no está en el PATH.

> Los scripts PowerShell deben tener extensión ".ps1".

>  El cmdlet `Write-Host` es equivalente a `echo`.

## El segundo script

* Los comentarios simples en PowerShell se definen con `#` (almohadilla).
* Desde la versión 2 de PowerShell se pueden usar bloques de comentarios con `<#` y `#>`.
* El script "comentarios.ps1":

```bash
# Soy un comentario simple en PowerShell
Get-Date
<#
	Y you soy un 
	bloque de comentarios
#>
query user
```

* Los bloques de comentarios se suelen utilizar para incluir ayuda en nuestros scripts, que luego podrá ser consultada con `Get-Help <script.ps1>`:

```powershell
<#
.SYNOPSIS
	Breve descripción de lo que hace nuestro script.
.DESCRIPTION
	Descripción detallada de nuestro script.
.NOTES
	File Name      : <script.ps1>
	Author         : Fran Vargas (mi@email.com)
	Prerequisite   : PowerShell vX sobre Windows X o superior.
	Copyright 2018 - Fran Vargas (IES Domingo Pérez Minik)
.LINK
	Script publicado en:
	http://fvarrui.github.io/ADD
.EXAMPLE
	Ejemplo 1
.EXAMPLE
	Ejemplo 2
#>
Write-Host "Sólo sirvo para mostrarte como se usan los comentarios"
```

* Si consultamos la ayuda del script anterior obtendremos los siguiente:

```powershell
PS> Get-Help .\comentariosv2.ps1

NOMBRE
    comentariosv2.ps1
    
SINOPSIS
    Breve descripción de lo que hace nuestro script.
    
SINTAXIS
    comentariosv2.ps1 [<CommonParameters>]
    
DESCRIPCIÓN
    Descripción detallada de nuestro script.

VÍNCULOS RELACIONADOS
    Script publicado en:
    http://fvarrui.github.io/ADD 

NOTAS
    Para ver los ejemplos, escriba: "get-help comentariosv2.ps1 -examples".
    Para obtener más información, escriba: "get-help comentariosv2.ps1 -detailed".
    Para obtener información técnica, escriba: "get-help comentariosv2.ps1 -full".
    Para obtener ayuda disponible en línea, escriba: "get-help comentariosv2.ps1 -online"

```

## El tercer script

* El script "educado.ps1":

```powershell
$nombre = Read-Host -Prompt "¿Cómo te llamas?"
Write-Host "Hola $nombre"
Write-Host "¿Cómo estás?"
```

> El cmdlet `Read-Host -Prompt <mensaje>` muestra el `mensaje` al usuario, luego espera a que se introduzca un dato desde teclado y éste es devuelto como un `String`, el cuál se almacena en la variable `$nombre`. 

* Es posible implementar el ejemplo anterior sin usar la variable:

```powershell
Write-Host "Hola $(Read-Host -Prompt "¿Cómo te llamas?")"
Write-Host "¿Cómo estás?"
```

## Paso de parámetros

* Es posible pasar parámetros a un script cuando lo ejecutamos, como si se tratara de un comando más.

### Bloque `param`

* Es recomendable declarar los parámetros al principio del script mediante la directiva `Param`.
* Sintaxis:

```powershell
Param(
	[<tipo>] $<nombre> [ = <valor> ],
	[...]
)
```

Donde:

| Elemento | Descripción                              |
| -------- | ---------------------------------------- |
| tipo     | Tipo de datos del parámetros (string, bool, int, ...). |
| nombre   | Nombre del parámetro (es como una variable más dentro del script). |
| valor    | Valor por defecto que tomará el parámetro si no se especifica al ejecutar el script. |

* Script "parametros.ps1":

```powershell
Param(
    [string] $primero,
    [string] $segundo = "valor por defecto",
    [switch] $tercero = $false,
    [bool] $cuarto,
    [int] $quinto
)
Write-Host $primero
Write-Host $segundo
Write-Host $tercero
Write-Host $cuarto
Write-Host $quinto
```

* Ejemplo de ejecución del script anterior:

```powershell
PS> Get-Help .\parametros.ps1
parametros.ps1 [[-primero] <string>] [[-segundo] <string>] [[-cuarto] <bool>] [[-quinto] <int>] [-tercero]

PS> .\parametros.ps1

valor por defecto
False
False
0

PS> .\parametros.ps1 -primero "hola" -tercero
hola
valor por defecto
True
False
0

PS> .\parametros.ps1 -primero "hola" -segundo "don pepito"
hola
don pepito
False
False
0

PS> .\parametros.ps1 -primero "hola" -tercero -quinto 123 -desconocido
hola
valor por defecto
True
False
123
```

#### Parámetro con conjunto de valores limitado

* Para hacer que un parámetro sólo pueda recibir un conjunto limitado de valores (limitado.ps1):

```powershell
Param(
    [ValidateSet("word","excel","powerpoint")][string] $parametro
)
Write-Host $parametro
```

* Uso del script:

```powershell
PS> .\limitado.ps1 -parametro word
word

PS> .\limitado.ps1 -parametro access
.\limitado.ps1 : No se puede validar el argumento del parámetro 'parametro'. El argumento "access" no pertenece al conjunto 
"word;excel;powerpoint" especificado por el atributo ValidateSet. Proporcione un argumento que pertenezca al conjunto e intente ejecutar el comando de nuevo.
En línea: 1 Carácter: 16
+ .\limitado.ps1 access
+                ~~~~~~
    + CategoryInfo          : InvalidData: (:) [limitado.ps1], ParameterBindingValidationException
    + FullyQualifiedErrorId : ParameterArgumentValidationError,limitado.ps1
```

#### Parámetros obligatorios

* Es posible hacer que un parámero sea obligatorio (mandatory.ps1):

```powershell
Param(
    [string] $primero,
    [Parameter(Mandatory=$true)][string] $segundo
)
Write-Host $primero
Write-Host $segundo
```

> En el ejemplo anterior, el parámetro `segundo` es obligatorio (**mandatory**).

* Si no especificamos el valor de los parámetros obligatorios, al ejecutar el script nos pedirá dichos valores:

```powershell
PS> .\mandatory.ps1
cmdlet mandatory.ps1 en la posición 1 de la canalización de comandos
Proporcione valores para los parámetros siguientes:
segundo: hola

hola
```

#### Tipos de datos de los parámetros

| Tipo     | Descripción                              |
| -------- | ---------------------------------------- |
| [int]    | Entero de 32 bits.                       |
| [long]   | Entero de 64 bits.                       |
| [string] | Cadena de caracteres.                    |
| [char]   | Carácter.                                |
| [bool]   | Verdadero (`$true`) o falso (`$false`).  |
| [double] | Número de coma flotante de 64 bits.      |
| [single] | Número de coma flotante de 32 bits.      |
| [switch] | Verdadero si se especifica el parámetro o falso en caso contrario. |

### Variables especiales

* Dentro de los scripts podemos usar las siguientes variables especiales:

| Variable      | Descripción                              |
| :------------ | :--------------------------------------- |
| $args         | Array con todos los parámetros  pasados al script (excluyendo los especificados en el bloque `Param`). Podemos acceder a los elementos del array con `$args[0]` , `$args[1]`, `$args[2] `, ..., `$args[n]`. |
| $args.Count   | Número de parámetros pasados al script (excluyendo los especificados en el bloque `Param`). |
| $LASTEXITCODE | Código de retorno de la última orden ejecutada (si es <> 0 indica que hubo un error). |

* Script "especiales.ps1":

```powershell
Write-Host "Primero:" $args[0]
Write-Host "Segundo:" $args[1]
Write-Host "Tercero:" $args[2]
Write-Host "Cuarto :" $args[3]
Write-Host "Número total de parámetros:" $args.Count

Get-Date
Write-Host "Valor de retorno del último comando:" $LASTEXITCODE
```

## El cmdlet `Read-Host`

* Sintaxis: `$variable = Read-Host [[-Prompt] <prompt>]`
* Donde:
  * `-Prompt <prompt>` muestra un mensaje antes de esperar a que se introduzca texto.
  * `variable` es la variable donde se guardará el texto introducido (**string**).
* Se utiliza para leer información desde el teclado (entrada estándar) y guardarla en variables.
* Script "leer-variable.ps1":

```powershell
<#
	Ejemplo de uso de Read-Host
#>
$var = Read-Host -Prompt "Introduce un valor"
Write-Host "El valor introducido es: $var"
```

## Operaciones aritméticas

* PowerShell integra de forma natural las operaciones aritméticas.

| Operador | Descripción                              | Ejemplo           |
| -------- | ---------------------------------------- | ----------------- |
| +        | Suma.                                    | `12 + 34`         |
| -        | Resta.                                   | `12 - 34`         |
| *        | Multiplicación.                          | `12 * 34`         |
| /        | División.                                | `34 / 12`         |
| %        | Resto de la división.                    | `34 % 12`         |
| ()       | Altera la precedencia de los operadores. | `( 12 + 34 ) * 5` |
| ++       | Incrementa en 1 el operando.             | `$variable++`     |
| --       | Decrementa en 1 el operando.             | `$variable--`     |

> Más información: `Get-Help about_Arithmetic_Operators`

* Script "aritmetica.ps1":

```powershell
$x = [int] (Read-Host -Prompt "Introduce el valor de la variable")
$y = ($x * 3)
$z = ($y / 2) + 5
Write-Host $z
```

> El cmdlet `Read-Host` devuelve valores de tipo cadena de caracteres (**string**) y para poder operar con esos valores debemos convertirlos primero a entero (**int**). 
>
> Para ello se especifica el tipo al que queremos convertir el valor entre corchetes (`[int]`). Este proceso de cambiar un valor de un tipo de datos a otro se llama **cast**. 

### La clase `Math`

* `Math` es una clase que ofrece funciones y constantes matemáticas (raíz cuadrada, potencia, número PI, ...). 
* Para acceder a sus miembros usamos `[Math]::<miembro>`.

| Miembro                      | Tipo      | Descripción                                    |
| ---------------------------- | --------- | ---------------------------------------------- |
| [Math]::PI                   | Constante | Devuelve el valor del número `PI` (3.1416...). |
| [Math]::Pow(base, exponente) | Función   | Devuelve la `base` elevada al `exponente`.     |
| [Math]::Sqrt(valor)          | Función   | Devuelve la raíz cuadrada de `valor`.          |
| [Math]::Max(valor1, valor2)  | Función   | Devuelve el mayor de `valor1` y `valor2`.      |
| [Math]::Min(valor1, valor2)  | Función   | Devuelve el menor de `valor1` y `valor2`.      |

* Script "radio.ps1":

```powershell
$radio = [double] (Read-Host -Prompt "Introduce el radio de la circunferencia")
$area = [Math]::PI * [Math]::pow($radio, 2)	# area = PI * radio^2
Write-Host "El área de la circunferencia de radio" $radio "es" $area
```

## Operadores relacionales

* Los operadores relacionales se utilizan para comparar valores y devuelven un valor de tipo booleano (True o False).

| Operador | Descripción                                                  | Ejemplo   |
| -------- | ------------------------------------------------------------ | --------- |
| `-eq`    | ¿Son los operandos iguales?                                  | `4 -eq 5` |
| `-ne`    | ¿Son los operandos distintos?                                | `4 -ne 5` |
| `-gt`    | ¿Es el primer operando mayor que el segundo operando?        | `4 -gt 5` |
| `-ge`    | ¿Es el primer operando mayor o igual que el segundo operando? | `4 -ge 5` |
| `-lt`    | ¿Es el primer operando menor que el segundo operando?        | `4 -lt 5` |
| `-le`    | ¿Es el primer operando menor o igual que el segundo operando? | `4 -le 5` |

> Para obtener más información podemos ejecutar el siguiente cmdlet: `Get-Help about_Comparison_Operators`

* Script "comparacion.ps1":

```powershell
Write-Host 
Write-Host "Compara de dos variables"
Write-Host "------------------------"
Write-Host 
$arg1 = [int] (Read-Host "Introduce la primera variable")
$arg2 = [int] (Read-Host "Introduce la segunda variable")
$resultado = $arg1 -eq $arg2
Write-Host "El resultado es" $resultado
```

* También se pueden comparar otros tipos de datos:
* Script "comparar-strings.ps1":

```powershell
# Compara dos strings
$a = "chuck"
$b = "norris"
$resultado = $a -eq $b
Write-Host "¿" $a "es igual a" $b "?" $resultado
```

* Otros operadores relacionales:

| Operador       | Descripción                                                  |
| -------------- | ------------------------------------------------------------ |
| `-in`          | Comprueba si un elemento está dentro de un array.            |
| `-notin`       | Comprueba si un elemento NO está dentro de un array.         |
| `-like`        | Compara una cadena con otra usando el metacarácter *. Si casa devuelve True. |
| `-notlike`     | Compara una cadena con otra usando el metacarácter *. Si casa devuelve False. |
| `-match`       | Compara una cadena de caracteres con una expresión regular. Si casa devuelve True. |
| `-notmatch`    | Compara una cadena de caracteres con una expresión regular. Si casa devuelve False. |
| `-contains`    | Comprueba si un array contiene un elemento.                  |
| `-notcontains` | Comprueba si un array NO contiene un elemento.               |
| `-is`          | Comprueba si un valor es de un tipo de datos determinado (int, double, string, ...) |
| `-isnot`       | Comprueba si un valor NO es de un tipo determinado.          |

* Ejemplos:

```powershell
# -in / -notin

PS> $miArray = "hola","don","pepito"
PS> "hola" -in $miArray
True
PS> "jose" -in $miArray
False
PS> "jose" -notin $miArray
True

# -like / -notlike

PS> "juan" -like "ju*"
True
PS> "pepe" -like "ju*"
False
PS> "pepe" -notlike "ju*"
True

# -match / -notmatch

PS> "12345678Z" -match "^[0-9]{8}\-?[a-zA-Z]$"	# exp. regular para validar un DNI
True

PS> "12345678-Z" -match "^[0-9]{8}\-?[a-zA-Z]$"
True

PS> "922102030" -match "^[0-9]{8}\-?[a-zA-Z]$"
False

PS> "922102030" -match "^[0-9]{9}$"		# exp. regular para validar teléfono con 9 dígitos
True

PS> "78560432E" -match "^[0-9]{9}$"
False

PS> "78560432E" -notmatch "^[0-9]{9}$"
True

# -contains / -notcontains

PS> $miArray = "hola","don","pepito"
PS> $miArray -contains "pepito"
True
PS> $miArray -contains "jose"
False
PS> $miArray -notcontains "jose"
True

# -is / -notis

PS> $a = 123
PS> $a -is [int]
True
PS> $a -is [string]
False
PS> $b = "hola"
PS> $b -is [int]
False
PS> $b -isnot [double]
True
PS> $b -is [string]
True
```



## Operadores lógicos

* Los operadores lógicos permiten crear expresiones de comparación más complejas.

| Operador | Descripción                                                  | Ejemplo             |
| -------- | ------------------------------------------------------------ | ------------------- |
| `-or`    | "O" lógico. Si alguno de los operandos es Verdadero, devuelve Verdadero. | `$False -or $True`  |
| `-and`   | "Y" lógico. Si alguno de los operandos es Falso, devuelve Falso. | `$False -and $True` |
| `-not`   | Negación. Si el operando es Verdadero, devuelve Falso, y viceversa. | `-not $True`        |

* Más ejemplos:

```powershell
PS> $a = $True
PS> $b = $False
PS> $a -or $b
True

PS> $a -and $b
False

PS> -not $a
False

PS> ($a -or $b) -and -not $true
False
```

## Estructuras de control

### if

* Sintaxis:

```powershell
if (condicion1) {
	ordenes
} elseif (condicion2) {
	ordenes
} else {
	ordenes
}
```

* Nos permite tomar decisiones según se cumplan o no unas condiciones (expresiones booleanas).
* El script "temperatura.ps1":

```powershell
$temperatura = [double] (Read-Host -Prompt "¿Cuál es la temperatura del paciente?")
if ($temperatura -lt 35) {
    "Hipotermia"
} elseif ($temperatura -gt 37 -and $temperatura -lt 38) {
    "Febrícula"
} elseif ($temperatura -ge 38 -and $temperatura -le 39) {
    "Fiebre"
} elseif ($temperatura -gt 39 -and $temperatura -lt 40.5) {
    "Fiebre alta"
} elseif ($temperatura -ge 40.5) {
    "Hipertermia"
} else {
    "Normal"
}
```

* El script "crea-dir.ps1":

```powershell
<#
    Comprueba si el directorio especificado por 
    parámetro existe, y si no lo crea.
#>
Param([Parameter(Mandatory=$true)][string] $directorio)
$ErrorActionPreference = "Stop"		# Hace que se pueda capturar la excepción
try {
    Get-Item $directorio
} catch [System.Management.Automation.ItemNotFoundException] {
    New-Item -ItemType Directory $directorio
}
```

### switch

* Sintaxis:

```powershell
switch (expresion) {
	patron1 { 
            ordenes
            break 
	}
	patron2 { 
			ordenes
			break 
	}
	[...]
	default { 
			ordenes 
	}
}
```

* Controla el flujo de ejecución basándose en la expresión dada. 
* La expresión se compara, en orden, con todos los patrones. 
* Cuando la expresión coincide con un patrón, se ejecutan todas las órdenes que vayan a continuación, hasta encontrar `break`.
* Ejemplo "dia-semana.ps1":

```powershell
$dia = Get-Date -UFormat "%a"
switch ($dia) {
    "lu." { "Hoy es lunes"; break }
    "ma." { "Hoy es martes"; break }
    "mi." { "Hoy es miércoles"; break }
    "ju." { "Hoy es jueves"; break }
    "vi." { "Hoy es viernes"; break }
    "sá." { "Hoy es sábado"; break }
    "do." { "Hoy es domingo"; break }
    default { "No sé que día es hoy" }
}
```

* Ejemplo "temperatura2.ps1":

```powershell
$temperatura = [double] (Read-Host -Prompt "¿Cuál es la temperatura del paciente?")
switch ($temperatura) {
    { $_ -lt 35 } # si la temperatura < 35
        { 
            "Hipotermia"
            break 
        }
    { $_ -gt 37 -and $_ -lt 38 } # si la temperatura > 37 y temperatura < 38
        {   
            "Febrícula"
            break
        }
    { $_ -ge 38 -and $_ -le 39 } # si la temperatura >= 38 y temperatura <= 39
        {   
            "Fiebre"
            break
        }
    { $_ -gt 39 -and $_ -lt 40.5 } # si la temperatura > 39 y temperatura < 40.5
        {   
            "Fiebre alta"
            break
        }
    { $_ -ge 40.4 } # si la temperatura >= 40.5
        {   
            "Hipertermia"
            break
        }
    default # resto de los casos
        {
            "Normal"
        }
}
```

> La variable `$_` se corresponde con el valor de la expresión. En el ejemplo anterior `$_`  es igual a `$temperatura`.

* Ejemplo "menu.ps1":

```powershell
Write-Host "1) Mostrar la fecha"
Write-Host "2) Mostrar la hora"
Write-Host "3) Comprobar conexión a Internet"
$opcion = [int] (Read-Host -Prompt "Elija una opción")
switch ($opcion) {
    1 { Get-Date -DisplayHint Date ; break }
    2 { Get-Date -DisplayHint Time ; break }
    3 { Test-Connection 8.8.8.8 ; break }
    default { "Opción desconocida" }
}
```

> Para obtener más información: `Get-Help about_Switch`

### while

* Sintaxis:

```powershell
while (condición) {
	orden(es)
}
```

* Se ejecutan las órdenes una y otra vez mientras se cumpla la condición.
* La condición se evalúa al principio del bucle.
* Ejemplo "mientras.ps1":

```powershell
<#
    Ejemplo de uso de while
#>
$a = 42
while ( $a –le 53 ) {
	Write-Host "Contador = $a"
	$a++		# $a = $a + 1
}
```

* Ejemplo "lineas.ps1":

```powershell
<#
	Va leyendo línea a línea el fichero indicado por parámetro
#>
Param([Parameter(Mandatory=$true)][string]$fichero)
$contenido = Get-Content $fichero	# devuelve array de string del contenido de fichero
$numero = 0
while ($numero -lt $contenido.Length) {
    Write-Host ($numero + 1) ":" $contenido[$numero]
    $numero++
}
Write-Host "El fichero tiene $numero líneas"
```

### do..while

- Sintaxis:

```powershell
do {
	orden(es)
} while (condicion)
```

- Se ejecutan las órdenes una y otra vez mientras se cumpla la condición.
- La condición se evalúa al final del bucle.

### do..until

* Sintaxis:

```bash
do {
	orden(es)
} until (condicion)
```

* Es similar a `do..while` sólo que en vez de ejecutar las órdenes "mientras" se cumpla la condición, las ejecuta HASTA (until) que se cumpla.
* La condiución se evalúa al final del bucle.
* Ejemplo "hasta.ps1":

```powershell
do {
    $palabra = Read-Host -Prompt "¿De qué color es el caballo blanco de Santiago?"
} until ($palabra -eq "blanco")
Write-Host -ForegroundColor Black -BackgroundColor White "¡Qué list@ eres!"
```

### for

* Sintaxis:

```powershell
for (inicializacion; condicion; actualizacion) {
	orden(es)
}
```

* El bucle se repite mientras se cumpla la **condición**.
* El bloque de **inicialización** se ejecuta una vez al principio. Se utiliza para inicializar variables, normalmente usadas como contadores.
* El bloque de **actualización** se ejecuta al final de cada iteración, después de ejecutar las órdenes.
* Ejemplo "bucle-for.ps1”:

```powershell
<#
    Ejemplo de uso de for:
    Cuenta desde el 42 hasta el 53
#>
for ($a = 42;  $a –le 53 ; $a++) {
	Write-Host "Contador = $a"
}
```

* Ejemplo "lineas2.ps1":

```powershell
<#
	Va leyendo línea a línea el fichero indicado por parámetro
#>
Param([Parameter(Mandatory=$true)][string]$fichero)
$contenido = Get-Content $fichero	# devuelve array de string del contenido de fichero
for ($numero = 0; $numero -lt $contenido.Length; $numero++) {
    Write-Host ($numero + 1) ":" $contenido[$numero]
}
Write-Host "El fichero tiene $numero líneas"
```

>  Para obtener más información: `Get-Help about_For`.

### foreach

* Sintaxis:

```powershell
foreach (elemento in lista) {
	orden(es)
}
```

* El bucle se repite tantas veces como elementos tiene la lista.
* En cada iteración, elemento toma el valor de un elemento de la lista.
* Script "ficheros.ps1":

```powershell
Param([string] $directorio = ".")
$ficheros = Get-ChildItem $directorio
foreach ($fichero in $ficheros) {
    Write-Host "El fichero" $fichero.Name "fue modificado el día" $fichero.LastWriteTime
}
```

* Script "lineas3.ps1":

```powershell
<#
	Va leyendo línea a línea el fichero indicado por parámetro
#>
Param([Parameter(Mandatory=$true)][string]$fichero)
$numero=0
foreach ($linea in Get-Content $fichero) {
    Write-Host ($numero + 1) ":" $linea
    $numero++
}
Write-Host "El fichero tiene $numero líneas"
```

> Para obtener más información: `Get-Help about_ForEach`.

### ForEach-Object

* Sintaxis:

```powershell
cmdlet | ForEach-Object { orden(es) }
```

* Es un cmdlet, no una estructura de control.
* Permite usarlo en combinación con otros cmdlets mediante tuberías.
* El bloque de órdenes se ejecuta por cada objeto devuelto por el cmdlet previo.
* Para recoger el elemento en cada iteración usamos la variable `$_`.
* Script "ficheros2.ps1":

```powershell
Param([string] $directorio = ".")
Get-ChildItem $directorio | ForEach-Object {
    Write-Host "El fichero" $_.Name "fue modificado por última vez el día" $_.LastWriteTime
}
```

* Script "lineas4.ps1":

```powershell
<#
	Va leyendo línea a línea el fichero indicado por parámetro
#>
Param([Parameter(Mandatory=$true)][string]$fichero)
$numero = 0
Get-Content $fichero | ForEach-Object {
    Write-Host ($numero + 1) ":" $_
    $numero++
}
Write-Host "El fichero tiene $numero líneas"
```

### break, continue y exit

#### `break`

* Hace que cualquier bucle termine y pase el control a la siguiente línea después del bloque de órdenes.

#### `continue`

* Hace que termine la iteración actual de cualquier bucle y pase directamente a la siguiente iteración, saltádose la ejecución de las órdenes restantes de la iteración actual.

#### `exit`

* Termina la ejecución del script.

## Funciones

* Las funciones permiten agrupar un conjunto de órdenes que se ejecutan con una cierta frecuencia, para poder reutilizarlas.
* Sintaxis de definición de una función:

```powershell
function nombre([parametro]...) {
	orden(es)
	[[return] valor]
}
```

* `nombre` es el nombre de la función. Lo utilizaremos para ejecutarla.

  > Es recomendable usar nombres formados por "Verbo-Sujeto" (por ejemplo: `Saludar-Persona`) y que el verbo utilizado sea alguno de los devueltos por el comando `Get-Verb`.

* `valor` es el resultado devuelto por la función, si es que devuelve algo.

* Las funciones se usan dentro del script como si fueran cmdlets.


- Para devolver un valor desde una función se hace enviando el resultado a la salida estándar (con un `Write-Host` por ejemplo).
- Podemos declarar tantas funciones como queramos dentro de un script.
- Podemos invocar unas funciones desde dentro de otras para crear funciones más complejas.
- Script "saludar.ps1":

```powershell
# Declaración de la función
function Saludar-Persona() {
    "Hola Don Pepito"
}

# Ejecución o invocación de la función
$saludo = Saludar-Persona

# Muetra el resultado devuelto por la función
Write-Host $saludo
```

#### Parámetros

* La función puede recibir parámetros al ejecutarla, y estos hay que declararlos entre los paréntesis `(...)`.
* La sintaxis para definir los parámetros de una función es idéntica al bloque `param` utilizado para especificar los parámetros de un script.
* Debemos tener en cuenta que los parámetros son variables locales dentro de la función, lo que significa que no se puede acceder a ellas desde fuera de la función.
* Ejemplo "saludar2.ps1":


```powershell
<# 
    Declara una función para recoger un nombre desde la consola
#>
function Obtener-Nombre() {
    return Read-Host -Prompt "Introduce un nombre"
}

<# 
    Declara la función que saluda a la persona con el nombre 
    especificado por parámetro.
#>
function Saludar-Persona([string] $nombre) {
    return "Hola " + $nombre
}

<# 
    Invoca a la función Obtener-Nombre y guarda el resultado 
    en la variable $nombre
#>
$nombre = Obtener-Nombre

<# 
    Invoca a la función Saludar pasándole el nombre 
    almacenado en la variable $nombre
#>
$saludo = Saludar-Persona $nombre

# Muetra el resultado devuelto por la función
Write-Host $saludo
```

#### Más ejemplos de funciones

* Ejemplo "sumar.ps1":

```powershell
function Calcular-AreaCirculo([double] $radio) {
	$area = [Math]::PI * $radio * $radio
	return $area
}

$radio = 25
$area = Calcular-AreaCirculo $radio
Write-Host "El área de una circunferencia de radio $radio es $area"
```

* Ejemplo "espacio-libre.ps1":

```powershell
# Devuelve la cantidad de bytes libres de la unidad indicada
function Obtener-EspacioLibre([string] $unidad) {
	return (Get-PSDrive -Name $unidad).Free
}

# Convierte la cantidad de bytes indicada en gigabytes
function Convertir-BytesAGigas([uint64] $cantidad) {
	return $cantidad / 1GB
}

$libre = Obtener-EspacioLibre "C" 
$gigas = Convertir-BytesAGigas $libre 
Write-Host "La unidad $unidad dispone de $gigas GB libres"
```

### Módulos

* Es posible crear módulos o librerías con elementos (llamados **miembros**) que podemos reutilizar desde otros scripts (variables, funciones, alias, cmdlets, ...).
* Los módulos son scripts con extensión `.psm1`.
* Módulo "trigonometria.psm1":

```powershell
function Calcular-AreaRectangulo([double] $ancho, [double] $alto) {
	return $ancho * $alto
}

function Calcular-AreaTriangulo([double] $base, [double] $altura) {
    return $base * $altura / 2
}

function Calcular-AreaCircunferencia([double] $radio) {
    return [Math]::PI * $radio * $radio
}
```

#### Exportar miembros de un módulo

* Es posible decidir lo que es accesible dentro de un módulo mediante el cmdlet `Export-ModuleMember`.
* Script "trigonometria-limitado.psm1":

```powershell
function Calcular-AreaRectangulo([double] $ancho, [double] $alto) {
	return $ancho * $alto
}

function Calcular-AreaTriangulo([double] $base, [double] $altura) {
    return $base * $altura / 2
}

function Calcular-AreaCircunferencia([double] $radio) {
    return [Math]::PI * $radio * $radio
}

Export-ModuleMember -Function Calcular-AreaRectangulo,Calcular-AreaTriangulo
```

* En el ejemplo anterior, los scripts que usen el módulo sólo podrán acceder a las funciones `Calcular-AreaRectangulo` y `Calcular-AreaTriangulo`, pero no a `Calcular-AreaCircunferencia`. Esta última será una función sólo para uso interno del módulo.

#### Usar un módulo

* Para usar un módulo desde un script es necesario importarlo con el cmdlet:

```powershell
Import-Module <modulo>
```

* `modulo` es el nombre del fichero del módulo con o sin la extensión.
* Para importar un módulo es necesario que se encuentre en alguna de las rutas de la variable de entorno `$env:PSModulePath` o especificar la ruta completa del módulo.
* El cmdlet `Import-Module` comprueba si los nombres de las funciones utilizan verbos válidos para PowerShell. Para conocer estos verbos disponemos del cmdlet `Get-Verb`.
  * Se mostrarán unos avisos en caso de que alguna función exportada no cumpla con este requisito.
  * Para deshabilitar esta comprobación usamos el parámetro `-DisableNameChecking`.

```powershell
Import-Module -DisableNameChecking <modulo>
```

* Por ejemplo, en el script "usar-modulo.ps1" vemos como importar y usar las funciones exportadas por el módulo "trigonometria.psm1":


```powershell
Import-Module -DisableNameChecking "x:\ruta\al\modulo\trigonometria" 
$area = Calcular-AreaRectangulo 12 34
Write-Host "Área = $area"
```


## Arrays (vectores)

* Definición de un array de cuatro elementos:

```powershell
PS> $miArray = "primero","segundo","tercero","cuarto"
```

* El array puede contener cuaquier tipo de elemtno
* Definición de un array fuertemente tipado:

```powershell
# Crea un array de enteros (int) con 4 elementos (del 0 al 3)
PS> $otroArray = New-Object int[] 4
PS> $otroArray[0] = 123
PS> $otroArray[1] = "hola"
No se puede convertir el valor "hola" al tipo "System.Int32". Error: "La cadena de entrada no tiene el formato correcto."
En línea: 1 Carácter: 1
+ $otroArray[1] = "hola"
+ ~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [], RuntimeException
    + FullyQualifiedErrorId : InvalidCastFromStringToInteger
```

* Acceder a uno de los elementos del array:

```powershell
PS> $miArray[1]
segundo
PS> $miArray[0] + " blablabla " + $miArray[2]
primero blablabla tercero
```

* Acceder a TODOS los elementos del array:

```powershell
PS> $miArray
primero
segundo
tercero
cuarto
```

* Conocer el número de elementos del array:

```powershell
PS> $miArray.Length
4 
```

