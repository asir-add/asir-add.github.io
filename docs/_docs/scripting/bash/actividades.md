---
title: Actividades de scripting en BASH
---

> En todos los scripts se debe comprobar si se está haciendo buen uso del script (número de parámetros) y mostrar una ayuda en caso contrario.

Actividades:

1. **sumar-dos.sh**: sumar dos números pasados por parámetro.

2. **multiplicar-dos.sh**: Multiplicar dos números pasados por parámetro.

3. **numero-mayor-de-3.sh**: comparar los tres números pasados por parámetro y devolver el mayor.

4. **yo-soy.sh**: mostrar tu nombre de usuario completo (recuperándolo de la variable `$USER`) en grande (con `figlet`).

5. **safedit.sh**: abre el editor `vi` (o `nano`) con el fichero pasado como argumento, de forma que justo antes de abrirlo haga una copia de seguridad del fichero abierto en el directorio `/tmp` con el nombre `$$.safedit.sh` (siendo `$$` la variable que contiene el PID del script).
   
   > MEJORA: Si se cierra el editor sin guardar, se borra la copia de seguridad.

6. **fecha-menor.sh**: comparar dos fechas en formato `DD-MM-AAAA` recibidas por parámetro y devolver la fecha menor.

7. **menu-tres.sh**: mostrar un menú que permita elegir entre los 3 primeros scripts (`sumar-dos.sh`, `multiplicardos. sh` y `numero-mayor-de-3.sh`), y que pida por teclado los datos que necesitan cada uno en sus argumentos para funcionar.

8. **cerrar-sistema.sh**: hacer el siguiente menú:
   
   ```
   a. Cerrar la sesión
   b. Reiniciar el sistema
   c. Apagar el sistema
   ```
   
   > NOTA 1: Matar la shell usando `kill` para la primera opción y usar el comando `shutdown` para las dos últimas.
   > 
   > NOTA 2: Al reiniciar y apagar se debe pedir por teclado el tiempo en segundos que tardará en llevarse a cabo la operación así como el mensaje de aviso a enviar a todos los usuarios que han iniciado sesión en nuestro sistema.

9. **menu-instalacion.sh**: hacer un menú que permite elegir entre desinstalar o instalar al menos 3 programas con `apt-get` (o `apt`). Si el programa está instalado, tendrá que mostrarse la opción **Desinstalar <programa>**. Si el
   programa no está instalado, se mostrará la opción **Instalar <programa>**.
   Por ejemplo, el programa `cowsay`.
   
   > NOTA: Averiguar cómo determinar si un programa ya está instalado en nuestro sistema, sin instalarlo.

10. **instalar-amsn.sh**: descargar, compilar e instalar AMSN (Alvaro’s Messenger), siguiendo los siguientes pasos:
    
    a. Descargar el código fuente AMSN usando el comando `wget <url>`, donde **url** es la dirección de lo que queremos descargar:
    
    [Download aMSN from SourceForge.net](http://sourceforge.net/projects/amsn/files/amsn/0.98.3/amsn-0.98.3-src.tar.gz/download)
    
    b. Descomprimimos y desempaquetamos el archivo descargado (usar `gunzip` y `tar`).
    
    c. Instalar los paquetes `g++`, `tcl-dev`, `tk-dev`, `libpng++-dev` y `libjpeg8-dev`.
    
    d. Dentro del directorio descomprimido y desempaquetado (`amsn-0.98.3/`) ejecutamos `./configure`.
    
    e. Compilamos el código fuente con el comando `make`.
    
    f. Instalamos el ejecutable como un comando más del sistema con `sudo make install`.
    
    g. Listo; para comprobar que funcionó nuestro script ejecutamos `amsn`.

11. **media-aritmetica.sh**: calcule la media aritmética de dos números introducidos por teclado. 
    
    >  OJO Utilizar el comando `bc` para hacer el cálculo, ya que permite operar con números reales. Ejemplo: `echo "2*3.14/5" | bc`

12. **srmd.sh**: muestre la suma, la resta, la multiplicación y la división de dos números que se introducen por teclado.

13. **cuadrado.sh**: calcule el área de un cuadrado cuyo lado es introducido por parámetro.

14. **rectangulo.sh**: calcule el área de un rectángulo cuyos lados son introducidos por parámetro.

15. **triangulo.sh**: calcule el área de un triángulo cuya base y altura son introducidos por parámetro.

16. **euro-ptas.sh**: realice la conversión de euros a pesetas.ptas-euros.sh: realice la conversión de pesetas a euros.

17. **precio-igic.sh**: pida el precio de un producto y el porcentaje de IGIC a aplicar, y que calcule el precio final.

18. **descuento.sh**: pida el precio de un producto y el porcentaje de descuento, y que calcule el precio final.

19. **particiones.sh**: muestre el espacio libre de cada partición.

20. **info-fich.sh**: pida por teclado el nombre de un fichero de texto y que a continuación muestre el contenido del fichero y después todos los datos del mismo (permisos, tamaño, etc.)

21. **frutiversa.sh**: pida 5 nombres de fruta y que luego los muestre en el orden inverso.

22. **listar-hasta-100.sh**: Listar los número del 1 al 100.

23. **listar-rango.sh**: Listar todos los números del rango comprendido entre los dos números pasados por parámetro.

24. **sumar-todos.sh**: Sumar todos los números pasados por parámetro y mostrar el resultado. Puede aceptar de 1 a N parámetros, pero no 0 parámetros (da error).

25. **sumar-rango.sh**: Sumar todos los números del rango comprendido entre los dos números pasados por parámetro, y mostrar el resultado.

26. **listar-usuarios.sh**: Listar los nombres de todos los usuarios del sistema (ver fichero `/etc/passwd`).

27. **listar-grupos.sh**: Listar los nombres de todos los grupos del sistema (ver fichero `/etc/group`).

28. **comprobar-todos.sh**: Comprobar que todos los ficheros pasados por parámetro existen y que son ficheros.

29. **config-ref.sh**: Mostrar la dirección IP y la máscara de subred del dispositivo o interfaz de red (lo, eth0, eth1, wlan0 wlan1, etc.) (ver comando `ifconfig` o `ip`). 
    
    Ejemplo:
    
    ```bash
    $ ./config_red.sh eth1
    Configuración de la interfaz eth1:
    * Dirección IP: 192.168.1.33
    * Máscara de subred: 255.255.255.0
    ```
    
    >  NOTA: Tener en cuenta el caso particular de la interfaz `lo`.

30. **espacio-ocupado.sh**: Calcular la cantidad de espacio total en bytes ocupado por todos los ficheros del directorio especificado por parámetro y mostrarla en la salida estándar. 
    
    Ejemplo:
    
    ```bash
    $ ./espacio-ocupado.sh /home/fran
    2600 bytes
    ```

31. **espacio-ocupado-plus.sh**: Mejorar el script anterior para que cumpla la siguiente sintaxis: `./espacio-ocupado.sh [ -b | -k | -m ] directorio(s)`
    
    Opciones:
    `-b` : mostrar el resultado en bytes
    `-k` : mostrar el resultado en kilobytes
    `-m` : mostrar el resultado en megabytes
    
    Además acepta 1 o más directorios y deberá devolver el total.

32. **descarga-masiva.sh**: Descargar, uno detrás de otro, todos los recursos especificados en un fichero de texto mediante sus URLs (ver comando “wget”).
    
    Ejemplo:
    
    ```bash
    $ cat direcciones.txt
    http://www.google.com
    http://www.iberia.es
    http://www.fsf.org
    $ ./multiwget.sh direcciones.txt
    Descargando http://www.google.com...ERROR
    Descargando http://www.iberia.es...OK
    Descargando http://www.fsf.org...OK
    Proceso completado:
    * Descargados: 2
    * Errores: 1
    ```

33. **saludar.sh**: Saludar según la hora del sistema: 
    
    a. Entre las [ 6h y las 12h ): **Buenos días**.
    
    b. Entre las [ 12h y las 19h ): **Buenas tardes**. 
    
    c. Entre las [ 19h y las 0h ): **Buenas noches**.
    
    d. Entre las [ 0h y las 6h ): **Vete a acostarte**

34. **superwrite.sh**: Enviar un mensaje con `write` a todos los usuarios que han iniciado sesión en el sistema, menos a ti mismo. Por ejemplo: 
    
    ```bash
    ./superwrite "Hola a tod@s, ¿qué tal?"
    ```

35. **crear-usuario.sh**: Pedir todos los datos del usuario por teclado y si no existe, crearlo (ver comando `useradd`). Los datos a solicitar son **nombre de usuario** (username), **shell** (por ejemplo, `/bin/bash`), **contraseña** (password) y **nombre completo** (por ejemplo, "Perico de los Palotes", a guardar en el comentario).
    
    > OJO: Se debe crear también el directorio HOME del usuario (consultar opciones de `useradd`).

36. **eliminar-usuario.sh**: Mostrar un listado sólo con los nombres de todos los usuarios regulares del sistema (`UID >= 1000`) y al elegir uno que se elimine completamente del sistema, junto con su directorio **HOME**. Para elegir un usuario se especificará su **UID**. Debe pedir confirmación antes de eliminarlo. En caso de que no exista, mostrar un mensaje de error.
