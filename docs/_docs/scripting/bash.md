---
title: Scripting en BASH
---

## El primer script

Para crear el script "Hola mundo" (holamundo.sh) abrimos un editor de texto (`nano`, p.ej.) y escribimos lo siguiente:

```bash
#!/bin/bash
echo Hola Mundo
```
Le damos permisos de ejecución:

```bash
$ chmod +x holamundo.sh
```

Y ya lo podemos ejecutar:

```bash
$ ./holamundo.sh
Hola Mundo
```

Hay que añadir `./` porque "holamundo.sh" no está en el PATH.

> La primera línea `#!/bin/bash` indica a la shell que el script debe ejecutarse con el intérprete de comandos `/bin/bash`. Debemos incluirla en todos los scripts.

## El segundo script

Los comentarios son todas aquellas líneas que comienzan por `#` (almohadilla). Son ignoradas por el intérprete de comandos.

El script "comentarios.sh":

```bash
#!/bin/bash
##########################
# Shell script de prueba #
##########################
who
date
```

## El tercer script

El script "educado.sh":

```bash
#!/bin/bash
read –p "¿Cómo te llamas? " nombre
echo Hola $nombre
echo ¿Cómo estás?
```

> El comando `read -p <mensaje> <nombre>` muestra el `mensaje` al usuario, luego espera a que se introduzca un dato desde teclado y éste se guarda en la variable `nombre`.

## Paso de parámetros

Es posible pasar parámetros a un script cuando lo ejecutamos, como si se tratara de un comando más.

Son accesibles utilizando las siguientes variables:

| Parámetro | Descripción                              |
| --------- | ---------------------------------------- |
| $0        | Corresponde con el nombre del script.    |
| $1        | Es el primer parámetro pasado al script. |
| $2        | Es el segundo parámetro pasado al script. |
| $3        | Es el tercer parámetro pasado al script. |
| $n        | Es el enésimo parámetro pasado al script. |

Script "parametros.sh":

```bash
#!/bin/bash
########################################
# Muestra los parámetros que le pasamos
# desde la línea de órdenes
########################################
echo Parámetro 0 = $0
echo Parámetro 1 = $1
echo Parámetro 2 = $2
echo Parámetro 3 = $3
```

Ejemplo de ejecución del script anterior:

```bash
$ ./parametros.sh abc 123 xyz
Parámetro 0 = ./parametros.sh
Parámetro 1 = abc
Parámetro 2 = 123
Parámetro 3 = xyz
```

## Variables especiales

Dentro de los scripts podemos usar las siguientes variables especiales:

| Parámetro | Descripción                              |
| :-------- | :--------------------------------------- |
| $#        | Número de parámetros pasados al script (excluyendo el nombre del script). |
| $?        | Código de retorno de la última orden ejecutada (si es <> 0 indica que hubo un error). |
| $*        | Cadena de parámetros entera (excluyendo el nombre del script). |
| $@        | Array de parámetros (excluyendo el nombre del script), donde cada parámetro es una cadena distinta dentro del array. |

Script "especiales.sh":

```bash
#!/bin/bash
########################################
# Muestra las variables especiales
########################################
echo La variable \# vale $#
echo La variable \* vale $*
cp
echo La variable \? vale $?
```

>  `\` (escapar): Se pone delante de los símbolos especiales para que no sean interpretados y se muestren literalmente.

## Ordenes importantes en la programación de scripts

### El comando `shift`

Sintaxis: `shift n`

Desplaza "n" posiciones a la izquierda los parámetros, de forma que, por ejemplo, el parámetro $3 pase a ser \$2 si n=1.

Esto permite leer los parámetros cuando son más de 9.

Script "desplazamiento.sh":

```bash
#!/bin/bash
######################################## 
# Ejemplo de uso de shift
########################################
echo Antes \$1 vale: $1
echo Antes \$2 vale: $2
echo Antes \$3 vale: $3
echo Antes \$# vale: $#
echo Antes \$* vale: $*
shift 2
echo Ahora \$1 vale: $1
echo Ahora \$2 vale: $2
echo Ahora \$3 vale: $3 
echo Ahora \$# vale: $#
echo Ahora \$* vale: $*
```

Ejemplo de ejecución del script anterior: `./desplazamiento uno dos tres cuatro cinco`

### El comando `read`

Sintaxis: `read [-p <prompt>] <variable(s)>`

Donde:
* `-p <prompt>` muestra un mensaje antes de esperar a que se introduzca texto.
* `variable(s)` es la lista de nombres de variables donde se guardará el texto introducido.

Se utiliza para leer información desde el teclado (entrada estándar) y guardarla en variables

Script "leer_variable.sh":

```bash
#!/bin/bash
########################################
# Ejemplo de uso de read
########################################
read -p "Introduce un valor: " var
echo El valor introducido es: $var
```

Script "leer_variables.sh":

```bash
#!/bin/bash
########################################
# Ejemplo de uso de read para leer
# varias variables de una vez
########################################
read -p "Introduce tres valores separados por espacios: " var1 var2 var3
echo El valor introducido para var1 es: $var1
echo El valor introducido para var2 es: $var2
echo El valor introducido para var3 es: $var3
```

Ejemplo de uso del script anterior:

```bash
$ ./leer_variables.sh hola don pepito, hola don jose
El valor introducido para var1 es: hola
El valor introducido para var2 es: don
El valor introducido para var3 es: pepito, hola don jose
```

### El comando `expr`

Sintaxis: `expr arg1 op arg2 [ op arg3 … ]`

Permite evaluar expresiones aritméticas, lógicas y relacionales (comparaciones).

#### Operadores aritméticos

| Operador | Descripción                             | Ejemplo         |
| -------- | --------------------------------------- | --------------- |
| +        | Suma arg1 y arg2.                       | `expr 12 + 34`  |
| -        | Resta arg1 y arg2.                      | `expr  12 - 34` |
| *        | Multiplica arg1 por arg2.               | `expr 12 \* 34` |
| /        | División entera entre arg1 y arg2.      | `expr 34 / 12`  |
| %        | Resto de la división entre arg1 y arg2. | `expr 34 % 12`  |

> :warning: Al multiplicar es necesario escapar el símbolo `*` con una `\` (barra invertida)

Script "sumar.sh":

```bash
#!/bin/bash
########################################
# Suma dos variables introducidas por teclado
########################################
echo
echo Suma de dos variables
echo ---------------------
echo
read -p "Introduce la primera variable: " arg1
read -p "Introduce la segunda variable: " arg2
resultado=$(expr $arg1 + $arg2)
# es lo mismo: resultado=`expr $arg1 + $arg2`
echo El resultado es $resultado
```

Es posible usar paréntesis en la expresión para alterar la prioridad de las operaciones, pero también hay que escaparlos con una `\` delante.

Script "calculos.sh":

```bash
#!/bin/bash
########################################
# Calcula: 2 * ( arg1 – arg2 )
########################################
read -p "Introduce la primera variable: " arg1
read -p "Introduce la segunda variable: " arg1
resultado=$(expr 2 \* \( $arg1 - $arg2 \))
echo El resultado es $resultado
```

> `*`, `(` y `)` son escapados con `\` porque tienen un significado especial para la BASH.

#### Operadores relacionales

| Operador | Descripción                              | Ejemplo        |
| -------- | ---------------------------------------- | -------------- |
| =        | ¿Son los operandos iguales?              | `expr 4 = 5`   |
| !=       | ¿Son los operandos distintos?            | `expr 4 != 5`  |
| >        | ¿Es el primer operando mayor que el segundo operando? | `expr 4 \> 5`  |
| >=       | ¿Es el primer operando mayor o igual que el segundo operando? | `expr 4 \>= 5` |
| <        | ¿Es el primer operando menor que el segundo operando? | `expr 4 \< 5`  |
| <=       | ¿Es el primer operando menor o igual que el segundo operando? | `expr 4 \<= 5` |

Si el resultado es verdadero devuelve 1, si es falso devuelve 0.

>  ​ `>` y `<` tienen significado especial para la BASH (son redirecciones) y hay que escaparlos.

Script "comparacion.sh":

```bash
#!/bin/bash
########################################
# Compara dos variables introducidas 
# por teclado
########################################
echo
echo Compara de dos variables
echo ------------------------
echo
read -p "Introduce la primera variable: " arg1
read -p "Introduce la segunda variable: " arg2
resultado=$(expr $arg1 = $arg2)
echo El resultado es $resultado
```

#### Operadores lógicos

| Operador | Descripción                              | Ejemplo       |
| -------- | ---------------------------------------- | ------------- |
| \|       | "O" lógico (OR). Si arg1 es dintinto de 0, el resultado es arg1; sino, el resultado es arg2. | `expr 0 \| 1` |
| &        | "Y" lógico (AND). Si arg1 y arg2 son distintos de 0, el resultado es arg1; sino, el resultado es arg2. | `expr 0 \& 1` |

>  :eyes: `|` y `&` deben ser escapados.

Más ejemplos:

```bash
$ expr 0 \& 2
0
$ expr 0 \& 0
0
$ expr 1 \& 1
1
$ expr 1 \| 3
1
$ expr 0 \| 3
3
```

### El comando `test`

#### Para archivos y directorios

Sintaxis: `test –<opcion> <argumento>` ó `[ –<opcion> <argumento> ]`

Donde:
* `opcion` indica el tipo de comprobación (test) que se va a realizar (ver tabla más abajo).
* `argumento` valor sobre el que se va a realizar la comprobación.

El comando `test` devuelve el resultado de la comprobación como un **valor de retorno**, que podemos consultar mediante la variable `$?`. A diferencia de `expr`, no muestra nada en la salida estándar.

Algunas opciones relacionadas con archivos y directorios:

| Opción | Descripción                              |
| ------ | ---------------------------------------- |
| -f     | Devuelve verdadero (0) si el archivo existe y es regular (no es directorio ni archivo de dispositivo). |
| -s     | Devuelve verdadero (0) si el archivo existe y tiene tamaño mayor que cero. |
| -r     | Devuelve verdadero (0) si el archivo existe y tiene permiso de lectura. |
| -w     | Devuelve verdadero (0) si el archivo existe y tiene permiso de escritura. |
| -x     | Devuelve verdadero (0) si el archivo existe y tiene permiso de ejecución. |
| -d     | Devuelve verdadero (0) si existe y es un directorio. |
| -e     | Devuelve verdadero (0) si existe.        |

Ejemplo:

```bash
# comprueba si existe el fichero /etc/passwd 
$ test -f /etc/passwd ; echo $?
0	# verdadero

$ test -f meloinvento ; echo $?
1	# falso

# comprueba si existe el directorio /etc
$ test -d /etc ; echo $?
0	# verdadero

# comprueba si existe el directorio /etc/passwd
$ test -d /etc/passwd ; echo $?
1	# falso
```

Igual que el ejemplo anterior pero usando la otra notación aceptada por la BASH para el comando `test`:

```bash
# comprueba si existe el fichero /etc/passwd 
$ [ -f /etc/passwd ] ; echo $?
0

# comprueba si existe el fichero meloinvento
$ [ -f meloinvento ] ; echo $?
1

# comprueba si existe el directorio /etc
$ [ -d /etc ] ; echo $?
0

# comprueba si existe el directorio /etc/passwd
$ [ -d /etc/passwd ] ; echo $?
1
```

#### Para cadenas de caracteres

Sintaxis: `test cadena1 operador cadena2` ó  `[ cadena1 operador cadena2 ]`

Operadores:

| Operador | Descripción                              |
| -------- | ---------------------------------------- |
| =        | Devuelve verdadero (0) si cadena1 es igual a cadena2, y falso (1) en caso contrario. |
| !=       | Devuelve verdadero (0) si cadena1 es distinta de cadena2. |
| <        | Devuelve verdadero (0) si cadena1 va antes alfabéticamente que cadena2. |
| >        | Devuelve verdadero (0) si cadena1 va después que cadena2. |

> `>` y `<` hay que escaparlos para que no sean interpretados por la BASH (son redirecciones) .

Es sensible a mayúsculas y minúsculas, por lo que “hola” es distinto de “Hola” u “HOLA”.

Ejemplos:

```bash
# comprueba si hola<>adios
$ [ "hola" != "adios" ] ; echo ?
0	# Verdadero

# comprueba si hola>adios
$ [ "hola" \> "adios" ] ; echo ?
0	# Verdadero

# comprueba si hola<adios
$ [ "hola" \< "adios" ] ; echo ?
1	# Falso

# comprueba si hola=adios
$ [ "hola" = "adios" ] ; echo ?
1	# Falso

# comprueba si hola=hola
$ [ "hola" = "hola" ] ; echo ?
0	# Verdadero

# comprueba si hola=HOLA
$ [ "hola" = "HOLA" ] ; echo ?
1	# Falso

```

Es conveniente poner las cadenas entre comillas dobles (""), sobre todo si tienen espacios. 

```bash
$ [ pepito = "pepito" ] ; echo $?
0
$ [ pepito perez = "juan perez" ] ; echo $?
bash: [: demasiados argumentos		# error
2
```

> Las variables también hay que ponerlas entre comillas dobles (""), para evitar errores en caso de que la variable no exista o esté vacía.
>
> ```bash
> [ "$var" = "hola" ]
> ```

#### Para números

Sintaxis: `test numero1 operador numero2  ` ó `[ numero1 operador numero2 ]`

Operadores:

| Operador | Descripción                              |
| -------- | ---------------------------------------- |
| -lt      | `numero1` menor que `numero2` (less than). |
| -le      | `numero1` menor o igual que `numero2` (less or equal) |
| -gt      | `numero1` mayor que `numero2` (greater than) |
| -ge      | `numero1` mayor o igual que `numero2` (greater or equal) |
| -eq      | `numero1` igual a `numero2` (equal)      |
| -ne      | `numero1` distinto de `numero2` (not equal) |

Ejemplos:

```bash
$ a=23

# comprueba si a<55
$ [ $a -lt 55 ] ; echo $?
0	# Verdadero

# comprueba si a<>23
$ test $a -ne 23 ; echo $?
1	# Falso
```

#### Operadores lógicos

Sintaxis: `test operando1 operador operando2` ó `[ operando1 operador operando2 ] `

Permite componer expresiones más complejas (AND, OR y NOT) con el comando `test`.

Operadores:

| Operador | Descripción                              |
| -------- | ---------------------------------------- |
| -a       | "Y" lógico (AND). Si `operando1` y `operando2`  son `true`, devuelve `true` (0), sino devuelve `false` (1). |
| -o       | "O" lógico (OR). Si `operando1` o `operando2` son `true`, devuelve `true` (0), sino devuelve `false` (1). |
| !        | Negación (NOT).  Invierte el operando. Si `operando1` es `true` (0), devuelve `false` (1), y si es `false` (<>0) devuelve `true` (0). |
| (...)    | Paréntesis. Permiten agrupar expresiones y alterar el orden de evaluación. :warning: Deben escaparse con `\`. |

>  `!` (NOT) es un operador unario: `test ! operando1`, por lo que sólo recibe un operando.

Ejemplos:

```bash
$ a=12
$ b=15

# comprueba si a=12 y b=15
$ [ $a -eq 12 -a $b -eq 15 ] ; echo $?
0

# comprueba si a=b o b=15
$ [ $a -eq $b -o $b -eq 15 ] ; echo $?
0

# comprueba si a<b y b=15 y no existe el fichero /etc/passwd
$ [ $a -lt $b -a $b -eq 15 -a ! -f /etc/passwd ] ; echo $?
1

# comprueba si a<b y b=15 y no existe el directorio /etc/passwd
$ [ $a -lt $b -a $b -eq 15 -a ! -d /etc/passwd ] ; echo $?
0

# comprueba si a<b y b=15 y existe el directorio /bin
$ [ $a -lt $b -a $b -eq 15 -a -d /bin ] ; echo $?
0

# comprueba si ( a<b y b=50 ) ó no existe el fichero /etc/passwd
$ [ \( $a -lt $b -a $b -eq 50 \) -o ! -f /etc/passwd ] ; echo $?
1
```

## Estructuras de control

### if

Sintaxis:

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

Nos permite tomar decisiones a partir de los códigos de retorno de los comandos utilizados como condiciones.

Normalmente se usa junto con la orden `test`.

El script "existe-hosts.sh":

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

El script "crea-dir.sh":

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

Podemos validar los parámetros pasados al script, como en el siguiente ejemplo ("comprobar.sh"):

```bash
#!/bin/bash
#######################################################
# Comprueba si existe el archivo pasado por parámetro, 
# y si existe indica de qué tipo es.
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

También podemos comprobar el resultado de la ejecución de otros programas.

Comprobar si existe un determinado usuario ("existe-usuario.sh"):

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

Ampliación del script anterior para averiguar además si el usuario es regular o del sistema ("existe-usuario2.sh"):

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

Sintaxis:

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

Controla el flujo de ejecución basándose en la "palabra" dada. 

La palabra se compara, en orden, con todos los patrones. 

Cuando la palabra coincida con un patrón, se ejecutan todas las órdenes que vayan a continuación, hasta encontrar `;;` (doble punto y coma).

Ejemplo "hoy.sh":

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

Ejemplo "letras.sh":

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

Variante del ejemplo anterior, donde acepta las letras tanto en mayúsculas como un minúsculas; ejemplo "letras-mayus-minus.sh":

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

Ejemplo "menu-comandos.sh":

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

Sintaxis:

```bash
while condición
do
	orden(es)
done
```

Se ejecutan las órdenes una y otra vez mientras se cumpla la condición.

La condición se evalúa a verdadero si es 0, y falso en caso contrario (como `if-fi`).

Como condición podemos utilizar el resultado de la ejecución de un comando, aunque lo normal es usar `test`.

Ejemplo "“mientras.sh":

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

Ejemplo "lineas.sh":

```bash
#!/bin/bash
##################################################
# Va leyendo línea a línea elfichero /etc/passwd
##################################################
numero=0
while read linea
do
	numero=$(expr $numero + 1)
	echo $numero $linea
done < /etc/passwd
echo "El fichero tiene $numero líneas"
```

### until

Sintaxis:

```bash
until condición
do
	orden(es)
done
```

Es similar a `while` sólo que en vez de ejecutar las órdenes "mientras" se cumpla la condición, las ejecuta HASTA (until) que se cumpla. Es decir, cuando la condición se cumple, termina.

Al igual que con `if` y con `while`, la condición es verdadera si vale 0 y falsa en caso contrario.

Como condición podemos utilizar el resultado de la ejecución de un comando, aunque normal es usar `test`.

Ejemplo "hasta.sh":

```bash
#!/bin/bash
until [ "$a" = hola ]
do
	read -p  "Introduce una cadena: " a
done
```

### for

Sintaxis:

```bash
for variable in lista
do
	orden(es)
done
```

El bucle se repite por cada una de las palabras o valores que contenga "lista". En cada iteración del bucle `for`, "variable" toma el valor del elemento correspondiente de “lista”.

Ejemplo "saludar-varios.sh":

```bash
#!/bin/bash
for i in Fran Árgel Paco Javi
do
	echo "Hola $i, ¿cómo estás?"
done
```

Ejemplo "comprobar-varios.sh":

```bash
#!/bin/bash
##########################################################
# Comprueba si existen los archivos pasados por parámetro, 
# y si existe indica de qué tipo es.
##########################################################
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

Hace que cualquier bucle `while`, `for` o `until` termine y pase el control a la siguiente línea después de `done`.

#### `continue`

Hace que termine la iteración actual de cualquier bucle `while`, `for` o `until` y pase directamente a la siguiente iteración sin terminar de ejecutar las órdenes de la iteración actual.

#### `exit [n]`

Termina la ejecución del script y devuelve `n` como código de retorno (0 significa éxito, distinto de 0 significa error)

### select

Sintaxis:

```bash
select i [ in lista ]
do
	orden(es)
done
```

Muestra un menú con las opciones de "lista". Cuando elijamos una opción,  `i` toma el valor de la opción seleccionada y se ejecutan las órdenes. El bucle `select` sólo termina con `break` o `exit`.

Es útil para hacer menús, combinándolo con un `case` anidado.

Ejemplo "elección.sh":

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

>  **PS3** es la variable de entorno que corresponde al prompt de `select`.

## Funciones

Sintaxis de definición de una función:

```bash
nombre() {
	orden(es)
}
```

Donde:

* `nombre` es el nombre de la función.

Las funciones permiten agrupar un conjunto de órdenes que se ejecutan con una cierta frecuencia, para poder reutilizarlas.

Podemos pasarle parámetros a la función al invocarla, y recogerlos dentro con `$1`, `$2`, ..., `$n`, así como conocer el número deparámetros con `$#` u obtenerlos todos con `$*`. Es como si el contenido de la función fuera un script dentro del script.

Las funciones se usan  dentro del script como si fueran comandos.

Para devolver un valor desde una función se puede hacer enviando el resultado a la salida estándar (con un `echo` por ejemplo).

Podemos declarar tantas funciones como queramos dentro de un script, así como invocar unas funciones desde dentro de otras para crear funciones más complejas.

Ejemplo "func-error.sh":

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

Ejemplo "func-sumar.sh":

```bash
#!/bin/bash
sumar() {
	echo $(expr $1 + $2)
}
resultado=$(sumar 12 4)
echo $resultado
```

Ejemplo "espacio-ocupado.sh":

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

Es posible crear una librería de funciones que podemos reutilizar desde otros scripts.

Librería "funciones.sh":

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

Script "espacio-ocupado.sh":

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

## Arrays (vectores)

Definición de un array de cuatro elementos

```bash
$ miArray=(primero segundo tercero cuarto)
```

Definición de un array añadiendo elemento a elemento:

```bash
$ miArray[0]=primero
$ miArray[1]=segundo
$ miArray[2]=tercero
$ miArray[3]=cuarto
```

Acceder a uno de los elementos del array:

```bash
$ echo ${miArray[1]}
segundo
$ echo ${miArray[0]} blablabla ${miArray[2]}
primero blablabla tercero
```

Acceder a TODOS los elementos del array:

```bash
$ echo El contenido del array es: ${miArray[*]}
El contenido del array es: primero segundo tercero cuarto
```

Conocer el número de elementos del array:

```bash
$ echo El número de elementos del array es: ${#miArray[*]}
El número de elementos del array es: 4
```

