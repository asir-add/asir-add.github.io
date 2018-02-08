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

#### Parámetros obligatorios

* Es posible hacer que un parámero sea obligatorio (mandatory.ps1):

```powershell
Param(
    [string] $primero,
    [Parameter(Mandatory=$true)] [string] $segundo
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

| Miembro                      | Descripción                              |
| ---------------------------- | ---------------------------------------- |
| [Math]::PI                   | Devuelve el valor de `PI`.               |
| [Math]::Pow(base, exponente) | Devuelve la `base` elevada al `exponente`. |
| [Math]::Sqrt(valor)          | Devuelve la raíz cuadrada de `valor`.    |
| [Math]::Max(valor1, valor2)  | Devuelve el mayor de `valor1` y `valor2`. |
| [Math]::Min(valor1, valor2)  | Devuelve el menor de `valor1` y `valor2`. |

* Script "radio.ps1":

```powershell
$radio = [double] (Read-Host -Prompt "Introduce el radio de la circunferencia")
$area = [Math]::PI * [Math]::pow($radio, 2)	# area = PI * radio^2
Write-Host "El área de la circunferencia de radio" $radio "es" $area
```

## Operadores relacionales

* Los operadores relacionales se utilizan para comparar valores y devuelven un booleano (True o False).

| Operador | Descripción                              | Ejemplo   |
| -------- | ---------------------------------------- | --------- |
| -eq      | ¿Son los operandos iguales?              | `4 -eq 5` |
| -ne      | ¿Son los operandos distintos?            | `4 -ne 5` |
| -gt      | ¿Es el primer operando mayor que el segundo operando? | `4 -gt 5` |
| -ge      | ¿Es el primer operando mayor o igual que el segundo operando? | `4 -ge 5` |
| -lt      | ¿Es el primer operando menor que el segundo operando? | `4 -lt 5` |
| -le      | ¿Es el primer operando menor o igual que el segundo operando? | `4 -le 5` |

> Más información: `Get-Help about_Comparison_Operators`

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

| Operador     | Descripción                              |
| ------------ | ---------------------------------------- |
| -in          | Comprueba si un elemento está dentro de un array. |
| -notin       | Comprueba si un elemento NO está dentro de un array. |
| -like        | Compara una cadena con otra usando el metacarácter *. Si casa devuelve True. |
| -notlike     | Compara una cadena con otra usando el metacarácter *. Si casa devuelve False. |
| -match       | Compara una cadena de caracteres con una expresión regular. Si casa devuelve True. |
| -notmatch    | Compara una cadena de caracteres con una expresión regular. Si casa devuelve False. |
| -contains    | Comprueba si un array contiene un elemento. |
| -notcontains | Comprueba si un array NO contiene un elemento. |
| -is          | Comprueba si un valor es de un tipo de datos determinado (int, double, string, ...) |
| -isnot       | Comprueba si un valor NO es de un tipo determinado. |

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

| Operador | Descripción                              | Ejemplo             |
| -------- | ---------------------------------------- | ------------------- |
| \-or     | "O" lógico. Si alguno de los operandos es Verdadero, devuelve Verdadero. | `$False -or $True`  |
| -and     | "Y" lógico. Si alguno de los operandos es Falso, devuelve Falso. | `$False -and $True` |
| -not     | Negación. Si el operando es Verdadero, devuelve Falso, y viceversa. | `-not $True`        |

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

<TODO>

### if

* Sintaxis:

```bash
if condicion1
then
	ordenes
elif condicion2
then
	ordenes
else 
	ordenes
fi
```

* Nos permite tomar decisiones a partir de los códigos de retorno de los comandos utilizados como condiciones.
* Normalmente se usa junto con la orden `test`.
* El script "existe-hosts.sh":

```bash
#!/bin/bash
#############################
# Ejemplo de uso de if-fi
#############################
if test –f /etc/hosts
then
	cat /etc/hosts
else
	echo El archivo no existe
fi
```

> Si el código de retorno del comando  `test` devuelve 0 (verdadero), se ejecuta la orden `cat`. 
>
> Si es <> 0, es falso y se ejecuta la orden  `echo`.

* El script "crea-dir.sh":

```bash
#!/bin/bash
#################################################
# Crea el directorio indicado por parámetro si no
# existe, y le da permisos sólo al propietario
#################################################
if [ ! –d $1 ]
then
	mkdir $1
	chmod 700 $1
fi
```

* Podemos validar los parámetros pasados al script, como en el siguiente ejemplo ("comprobar.sh"):

```bash
#!/bin/bash
#######################################################
# Comprueba si existe el archivopasado por parámetro, 
# y si existe indica de que tipoes.
#######################################################

# comprueba si el número de parámetros ($#) pasados al script es 0
if [ $# -eq 0 ]; then
	echo Debes introducir al menos un argumento.
	exit 1
fi

if [ -f "$1" ]; then
	# es un archivo regular
	echo –n "$1 es un archivo regular "
	if [ -x $1 ]; then
		echo "ejecutable"
	else
		echo "no ejecutable"
	fi
elif [ -d "$1" ]; then
	# es un directorio
		echo "$1 es un directorio"
else
	# es una cosa rara
	echo "$1 es una cosa rara o no existe"
fi
```

* También podemos comprobar el resultado de la ejecución de otros programas.
* Comprobar si existe un determinado usuario ("existe-usuario.sh"):

```bash
#!/bin/bash
if grep –q "^$1:" /etc/passwd
then
	echo El usuario $1 ya existe en el sistema.
else
	echo El usuario $1 no existe en el sistema.
fi
```

> La opción `-q` de `grep` hace que el resultado no salga en la consola, sino que simplemente el comando devuelva 0 (verdadero) si lo encuentra y <> 0 (falso) en caso contrario.

* Ampliación del script anterior para averiguar además si el usuario es regular o del sistema ("existe-usuario2.sh"):

```bash
#!/bin/bash
if grep –q "^$1:" /etc/passwd
then
	echo El usuario $1 ya existe en el sistema.
	IDU=$(cat/etc/passwd | grep "^$1:" | cut –f 3 –d)
	if [ $IDU –ge 500 ]
	then
		echo $1 es un usuario regular
	else
		echo $1 es un usuario del sistema
	fi
else
	echo Elusuario $1 no existe en el sistema.
fi 
```

### case

* Sintaxis:

```bash
case palabra in
	patron1)    
		orden1
		;;
	patron2)    
		orden2
		;;
	[...]
	patronN)    
		ordenN
		;;
esac
```

* Controla el flujo de ejecución basándose en la "palabra" dada. 
* La palabra se compara, en orden, con todos los patrones. 
* Cuando la palabra coincida con un patrón, se ejecutan todas las órdenes que vayan a continuación, hasta encontrar `;;` (doble punto y coma).
* Ejemplo "hoy.sh":

```bash
#!/bin/bash
#############################
# Ejemplo de uso de case-esac
#############################
dia=$(date | cut –c 1-3)
case $dia in
	lun)   echo Hoy es lunes ;;
	mar)   echo Hoy es martes ;;
	mié)   echo Hoy es miércoles ;;
	jue)   echo Hoy es jueves ;;
	vie)   echo Hoy es viernes ;;
	sáb)   echo Hoy es sábado ;;
	dom)   echo Hoy es domingo ;;
	*)     echo No se sabe qué es hoy ;;
esac
```

* Ejemplo "letras.sh":

```bash
#!/bin/bash
read -p "Introduzca A, B o C: " letra
case "$letra" in
	A)
		echo Introdujo A
		;;
	B)
		echo Introdujo B
		;;
	C)
		echo Introdujo C
		;;
	*)     
		echo No introdujo A, B o C
		;;
esac
```

* Variante del ejemplo anterior, donde acepta las letras tanto en mayúsculas como un minúsculas; ejemplo "letras-mayus-minus.sh":

```bash
#!/bin/bash
read -p "Introduzca A, B o C: " letra
case "$letra" in
	a|A)
		echo Introdujo A
		;;
	b|B)
		echo Introdujo B
		;;
	c|C)
		echo Introdujo C
		;;
	*)     
		echo No introdujo A, B o C
		;;
esac
```

* Ejemplo "menu-comandos.sh":

```bash
#!/bin/bash
################################
# Menú para comandos sencillos
################################
echo –e "\n --- MENU DE COMANDOS---\n"
echo " a. Fecha y hora actual"
echo " b. Usuarios conectados"
echo " c. Nombre del directorio detrabajo"
echo " d. Contenidos del directoriode trabajo"
echo
read -p "Introduzca una opción: " opcion
echo
case "$opcion" in
	a)     date ;;
	b)     who ;;
	c)     pwd ;;
	d)     ls ;;
	*)     echo Opción incorrecta ;;
esac 
```

### while

* Sintaxis:

```bash
while condición
do
	orden(es)
done
```

* Se ejecutan las órdenes una y otra vez mientras se cumpla la condición.
* La condición se evalúa a verdadero si es 0, y falso en caso contrario (como `if-fi`).
* Como condición podemos utilizar el resultado de la ejecución de un comando, aunque lo normal es usar `test`.
* Ejemplo "“mientras.sh":

```bash
#!/bin/bash
###################################
# Ejemplo de uso de while
###################################
a=42
while [ $a –le 53 ]
do
	echo Contador = $a
	a=$(expr $a + 1)
done
```

* Ejemplo "líneas.sh":

```bash
#!/bin/bash
##################################################
# Va leyendo línea a línea elfichero /etc/passwd
##################################################
numero=1
while read linea
do
	echo $numero $linea
	numero=$(expr $numero + 1)
done < /etc/passwd
echo "El fichero tiene $numero líneas"
```

### until

* Sintaxis:

```bash
until condición
do
	orden(es)
done
```

* Es similar a `while` sólo que en vez de ejecutar las órdenes "mientras" se cumpla la condición, las ejecuta HASTA (until) que se cumpla. Es decir, cuando la condición se cumple, termina.
* Al igual que con `if` y con `while`, la condición es verdadera si vale 0 y falsa en caso contrario.
* Como condición podemos utilizar el resultado de la ejecución de un comando, aunque normal es usar `test`.
* Ejemplo "hasta.sh":

```bash
#!/bin/bash
until [ "$a" = hola ]
do
	read -p  "Introduce una cadena: " a
done
```

### for

* Sintaxis:

```bash
for variable in lista
do
	orden(es)
done
```

* El bucle se repite por cada una de las palabras o valores que contenga "lista".
* En cada iteración del bucle `for`, "variable" toma el valor del elemento correspondiente de “lista”.
* Ejemplo “saludar-varios.sh”:

```bash
#!/bin/bash
for i in Fran Árgel Paco Javi
do
	echo "Hola $i, ¿cómo estás?"
done
```

* Ejemplo "comprobar-varios.sh":

```bash
#!/bin/bash
#######################################################
# Comprueba si existen los archivospasados por parámetro, 
# y si existe indica de que tipoes.
#######################################################
if [ $# -eq 0 ]; then
	echo Debes introducir al menos un argumento.
	exit 1
fi

for fichero in $*
do
	if [ -f "$fichero" ]; then
		# es un archivo regular
		echo –n "$fichero es un archivo regular "
		if [ -x $fichero ]; then
			echo "ejecutable"
		else
			echo "no ejecutable"
		fi
	elif [ -d "$fichero" ]; then
		# es un directorio
		echo "$fichero es un directorio"
	else
		# es una cosa rara
		echo "$fichero es una cosa rara o no existe"
	fi
done 
```

### break, continue y exit

#### `break`

* Hace que cualquier bucle `while`, `for` o `until` termine y pase el control a la siguiente línea después de `done`.

#### `continue`

* Hace que termine la iteración actual de cualquier bucle `while`, `for` o `until` y pase directamente a la siguiente iteración sin terminar de ejecutar las órdenes de la iteración actual.

#### `exit [n]`

* Termina la ejecución del script y devuelve `n` como código de retorno (0 significa éxito, distinto de 0 significa error)

### select

* Sintaxis:

```bash
select i [ in lista ]
do
	orden(es)
done
```

* Muestra un menú con las opciones de “lista”.
* Cuando elijamos una opción,  `i` toma el valor de la opción seleccionada y se ejecutan las órdenes.
* Está bien para hacer menús, combinándolo con un `case` anidado.
* El bucle `select` sólo termina con `break` o `exit`.
* Ejemplo "elección.sh":

```bash
#!/bin/bash
PS3="Elije una opción: "
select i in Listado Quien Salir
do
	case $i in
		Listado)     ls –l ;;
		Quien)       who ;;
		Salir)       exit 0 ;;
		*)           echo “Eeehhh???” ;;
	esac
done
```

>  PS3 es la variable de entorno que corresponde al prompt de `select`.

## Funciones

* Sintaxis de definición de una función:

```bash
nombre() {
	orden(es)
}
```

* Las funciones permiten agrupar un conjunto de órdenes que se ejecutan con una cierta frecuencia, para poder reutilizarlas.
* En "nombre" es el nombre de la función.
* Podemos pasarle parámetros a la función al invocarla, y recogerlos dentro con `$1`, `$2`, ..., `$n`, así como conocer el número deparámetros con `$#` u obtenerlos todos con `$*`. Es como si el contenido de la función fuera un script dentro del script.
* Las funciones se usan  dentro del script como si fueran comandos.


* Para devolver un valor desde una función se puede hacer enviando el resultado a la salida estándar (con un `echo` por ejemplo).
* Podemos declarar tantas funciones como queramos dentro de un script.
* Podemos invocar unas funciones desde dentro de otras para crear funciones más complejas.
* Ejemplo "func-error.sh":

```bash
#!/bin/bash

# definimos la función "error"
error() {
	echo Error de sintaxis
	exit 1
}

if [ $# -eq 0 ]; then
	error               # Invocación de la función "error"
else
	echo Hay $# argumentos
fi
```

* Ejemplo "func-sumar.sh":

```bash
#!/bin/bash
sumar() {
	echo $(expr $1 + $2)
}
resultado=$(sumar 12 4)
echo $resultado
```

* Ejemplo "espacio-ocupado.sh":

```bash
#!/bin/bash
ocupado() {
	particion=$1
	df -k | grep /dev/$particion | tr –s " " | cut –d " " –f 3
}

kbToMb() {
	kilobytes=$1
	resultado=$(expr $kilobytes / 1024)
	echo $resultado
}

partSda1=$(ocupado sda1)
partSda2=$(ocupado sda2)
partSdb1=$(ocupado sdb1)

total=$(expr $partSda1 + $partSda2 + $partSdb1)
totalMegas=$(kbToMb $total)

echo "El espacio ocupado total esde $totalMegas MB"
```

>  `df -k` devuelve el espacio libre y ocupado en kilobytes (-k) por las distintas particiones montadas.

### Librería de funciones

* Es posible crear una librería de funciones que podemos reutilizar desde otros scripts.
* Librería "funciones.sh":

```bash
ocupado() {
	particion=$1
	df -k | grep /dev/$particion | tr –s " " | cut –d " " –f 3
}
kbToMb() {
	kilobytes=$1
	resultado=$(expr $kilobytes / 1024)
	echo $resultado
}
```

* Script "espacio-ocupado.sh":

```bash
#!/bin/bash

# incluir el script con las funciones para poder utilizarlas
. ./funciones.sh

partSda1=$(ocupado sda1)
partSda2=$(ocupado sda2)
cpartSdb1=$(ocupado sdb1)

total=$(expr $partSda1 + $partSda2 + $partSdb1)
totalMegas=$(kbToMb $total)

echo "El espacio ocupado total esde $totalMegas MB"
```

:information_source: Para incluir un script dentro de otro ponemos un punto (.) un espacio y la ruta completa al script a incluir.

`. /path/to/libreria`

También es válida la siguiente forma:

`source /path/to/libreria`

## Definición y uso de arrays (vectores)

* Definición de un array de cuatro elementos

```bash
$ miArray=(primero segundo tercero cuarto)
```

* Definición de un array añadiendo elemento a elemento:

```bash
$ miArray[0]=primero
$ miArray[1]=segundo
$ miArray[2]=tercero
$ miArray[3]=cuarto
```

* Acceder a uno de los elementos del array:

```bash
$ echo ${miArray[1]}
segundo
$ echo ${miArray[0]} blablabla ${miArray[2]}
primero blablabla tercero
```

* Acceder a TODOS los elementos del array:

```bash
$ echo El contenido del array es: ${miArray[*]}
El contenido del array es: primero segundo tercero cuarto
```

* Conocer el número de elementos del array:

```bash
$ echo El número de elementos del array es: ${#miArray[*]}
El número de elementos del array es: 4
```

