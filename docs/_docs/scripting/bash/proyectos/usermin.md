---
title: Proyecto USERMIN
---

## Descripción

A veces la gestión de los usuarios mediante los comandos del sistema se vuelve un poco tediosa y que requiere recordar los comandos que hay que usar así sus distintas opciones. 

Este proyecto consiste en elaborar un script para la Shell BASH en GNU/Linux que nos facilite la administración de los usuarios del sistema:

* El nombre del script será `useradmin.sh`.

## Sintaxis

* La sintaxis del script es la siguiente:

```bash
useradmin.sh [ --help | --listar | --consultar usuario | --nuevo usuario | --eliminar usuario | -f fichero ]
```

* El funcionamiento del script será el siguiente:

| Opción                | Descripción                              |
| --------------------- | ---------------------------------------- |
| `usuario`             | Es el nombre del usuario a consultar, crear o eliminar. |
| `--help`              | Muestra la ayuda, explicando para qué sirve el script, sus distintas opciones y cómo se utiliza. |
| `--listar`            | Muestra un listado con los nombres, UID y Shell de todos los usuarios del sistema del 1000 en adelante, separando los campos con una coma (`,`). |
| `--consultar usuario` | Muestra los datos del usuario especificado en forma de ficha (nombre de usuario,UID, GID, Shell y directorio HOME). |
| `--nuevo usuario`     | Crea un nuevo usuario, pidiendo por teclado uno a uno los datos del nuevo usuario; los datos que se pedirán son "comentario", "shell" y "contraseña". |
| `--eliminar usuario`  | Elimina un usuario existente. Se deberá comprobar si el usuario existe; y si no existe, devolver un mensaje de error. Asimismo, se deberá pedir confirmación al usuario (Sí/No) antesde eliminarlo. |
| `-f fichero`          | Crear usuarios por lotes, a partir de los datos especificados en un fichero. Se deberá leer línea a línea el fichero especificado, conteniendo cada línea la información de un usuario diferente. El formato de cada línea será el siguiente (los campos van separados por comas `,`): `usuario,contraseña,shell,comentario` |

> Para crear los usuarios usa el comando `useradd`; no olvides la opción `-m` para que se cree automáticamente su directorio HOME.

> Para establecer la contraseña de forma automática:
> `$ echo -e "contra\ncontra" | sudo passwd usuario`
> Donde "contra" es la nueva contraseña y "usuario" es el usuario al que se quiere poner la
> misma.

> Para eliminar los usuarios usar el comando `userdel`; no olvides la opción `-r` para que se elimine también su directorio HOME.

## Ejemplos de uso

```bash
$ useradmin.sh --listar
fran,1000,/bin/bash,Fran Vargas
chanquete,1001,/bin/sh,El barco de Chanquete
juancho,1164,/bin/ksh,Lagarto Juancho
$ useradmin.sh --consultar fran
Usuario: fran
UID: 1000
GID: 1001
Shell: /bin/bash
HOME: /home/fran

```

## Ejemplo de fichero de usuarios

Un ejemplo de fichero para la opción `-f` del script sería el siguiente:

```
michael,nait,/bin/bash,Michael Night
pirana,1234,/bin/bash,Pirañita
messi,gol,/bin/sh,Leo Messi
```

## Recorrer el fichero

La forma de recorrer el fichero línea a línea es la siguiente:

```bash
while read linea
do
	# aquí se procesa la “linea” leída del fichero con los ...
	# datos del usuario usando “cut”, “tr”, etc., según se necesite
	# y se van creando los usuarios uno tras otro (por cada línea,
	# un usuario)
done < fichero_a_leer
```

> Se tendrá en cuenta que se verifiquen los parámetros especificados por el usuario y se controlen, en la medida de lo posible, los errores.

> El script se tendrá que ejecutar como "root" para poder manipular los usuarios.

## Calificación

| Apartado           | Funcionalidad                            | Peso (%) |
| ------------------ | ---------------------------------------- | :------: |
| Opción --help      | Mostrar la ayuda del comando.            |    10    |
| Opción --listar    | Listar todos los usuarios desde el UID 1000 en adelante. |    15    |
| Opción --consultar | Mostrar la ficha de un usuario concreto. |    15    |
| Opción --nuevo     | Crear un nuevo usuario.                  |    20    |
| Opción --eliminar  | Eliminar un usuario existente.           |    20    |
| Opción -f          | Crear usuarios por lotes desde un fichero. |    20    |