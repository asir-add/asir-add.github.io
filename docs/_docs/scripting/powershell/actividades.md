---
title: Actividades de scripting en PowerShell
---

> En todos los scripts se debe comprobar si se está haciendo buen uso del script (número de parámetros) y mostrar una ayuda en caso contrario.

Actividades:

1. **Sumar-Dos.ps1**: sumar dos números pasados por parámetro.

2. **Multiplicar-Dos.ps1**: Multiplicar dos números pasados por parámetro.

3. **Numero-MayorDe3.ps1**: comparar los tres números pasados por parámetro y devolver el mayor.

4. **Yo-Soy.ps1**: mostrar tu nombre de usuario completo (recuperándolo de la variable `$env:USERNAME`).

5. **Safedit.ps1**: abre el `notepad` con el fichero pasado como argumento, de forma que justo antes de abrirlo haga una copia de seguridad del fichero abierto en el directorio `$env:TEMP` con el nombre `${fichero}.backup` (siendo `$fichero` el nombre del fichero pasado por parámetro).

6. **Fecha-Menor.ps1**: comparar dos fechas en formato `DD-MM-AAAA` recibidas por parámetro y devolver la fecha menor.

7. **Menu-Tres.ps1**: mostrar un menú que permita elegir entre los 3 primeros scripts (`Sumar-Dos.ps1`, `Multiplicar-Dos.ps1` y `Numero-MayorDe3.ps1`), y que pida por teclado los datos que necesitan cada uno en sus argumentos para funcionar.

8. **Cerrar-Sistema.ps1**: hacer el siguiente menú:
   
   ```
   a. Reiniciar el sistema
   b. Apagar el sistema
   c. Salir
   ```
   
   > NOTA: Consultar la ayuda de los comandos `Restart-Computer` y `Stop-Computer`.

9. **Menu-Instalacion.ps1**: hacer un menú que permite elegir entre desinstalar o instalar al menos 3 programas con `choco` (Chocolatey). Si el programa está instalado, tendrá que mostrarse la opción **Desinstalar "programa"**. Si el
   programa no está instalado, se mostrará la opción **Instalar "programa"**.
   
   > NOTA: Averiguar cómo determinar si un programa ya está instalado en nuestro sistema, sin instalarlo.

10. **Media-Aritmetica.ps1**: calcule la media aritmética de `n` números introducidos por parámetro. 
    
    > Ejemplo: 
    > 
    > ```powershell
    > PS> ./Media-Aritmetica.ps1 5 7.5 12 9 4
    > 7.5
    > ```

11. **SRMD.ps1**: muestre la suma, la resta, la multiplicación y la división de dos números que se introducen por teclado.

12. **Cuadrado.ps1**: calcule el área de un cuadrado cuyo lado es introducido por parámetro.

13. **Rectangulo.ps1**: calcule el área de un rectángulo cuyos lados son introducidos por parámetro.

14. **Triangulo.ps1**: calcule el área de un triángulo cuya base y altura son introducidos por parámetro.

15. **Euro-To-Pesetas.ps1**: realice la conversión de la cantidad en euros pasada por parámetro a pesetas.

16. **Pesetas-To-Euros.ps1**: realice la conversión de la cantidad en pesetas pasada por parámetro a euros.

17. **Calcular-IGIC.ps1**: pida por teclado el precio de un producto y el porcentaje de IGIC a aplicar, y que calcule el precio final.

18. **Descuento.ps1**: pida por teclado el precio de un producto y el porcentaje de descuento, y que calcule el precio final.

19. **Get-InfoFich.ps1**: pida por teclado el nombre de un fichero de texto y que a continuación muestre el contenido del fichero y después todos los datos del mismo (propietario, tamaño, fecha de última modificación, fecha de creación, etc.)

20. **Reverse.ps1**: introduzca por teclado un número `n` de palabras y que luego las muestre en el orden inverso.

21. **Listar-Hasta100.ps1**: Listar los número del 1 al 100.

22. **Listar-Rango.ps1**: Listar todos los números del rango comprendido entre los dos números pasados por parámetro.

23. **Sumar-Todos.ps1**: Sumar todos los números pasados por parámetro y mostrar el resultado. Puede aceptar de 1 a N parámetros, pero no 0 parámetros (da error).

24. **Sumar-Rango.ps1**: Sumar todos los números del rango comprendido entre los dos números pasados por parámetro, y mostrar el resultado.

25. **Listar-Usuarios.ps1**: Listar sólo los nombres de los usuarios del sistema.

26. **Listar-Grupos.ps1**: Listar sólo los nombres de todos los grupos del sistema y los miembros de cada grupo.

27. **Comprobar-Todos.ps1**: Comprobar que todos los ficheros pasados por parámetro existen y que son ficheros.

28. **Config-Red.ps1**: Mostrar la dirección IP, la máscara de subred y los servidores DNS de la interfaz de red indicada por parámetro (user el alias de la interfaz de red).
    
    Ejemplo:
    
    ```powershell
    PS> .\Config-Red.ps1 Ethernet
    Configuración de la interfaz Ethernet:
    * Dirección IP: 192.168.1.33
    * Máscara de subred: 255.255.255.0
    * DNS: 8.8.8.8 172.17.198.5
    ```

29. **Espacio-Ocupado.ps1**: Calcular la cantidad de espacio total en bytes ocupado por todos los ficheros del directorio especificado por parámetro y mostrarla en la salida estándar. 
    
    Ejemplo:
    
    ```powershell
    PS> .\Espacio-Ocupado.ps1 C:\Users\fvarrui
    626809 bytes
    ```

30. **Espacio-OcupadoPlus.ps1**: Mejorar el script anterior para que cumpla la siguiente sintaxis: `Espacio-Ocupado.ps1 [ -Bytes | -KB | -MB ] directorio(s)`
    
    Opciones:
    `-Bytes` : mostrar el resultado en bytes
    `-KB` : mostrar el resultado en kilobytes
    `-MB` : mostrar el resultado en megabytes
    
    Además acepta 1 o más directorios y deberá devolver el total.

31. **Descarga-Masiva.ps1**: Descargar, uno detrás de otro, todos los recursos especificados en un fichero de texto mediante sus URLs (ver comando `Invoke-WebRequest` para descargar ficheros a partir de su URL).
    
    Ejemplo:
    
    ```powershell
    PS> Get-Content direcciones.txt
    http://www.google.com
    http://www.iberia.es
    http://www.fsf.org
    PS> .\Descarga-Masiva.ps1 direcciones.txt
    Descargando http://www.google.com...ERROR
    Descargando http://www.iberia.es...OK
    Descargando http://www.fsf.org...OK
    Proceso completado:
    * Descargados: 2
    * Errores: 1
    ```

32. **Saludar.ps1**: Saludar según la hora del sistema: 
    
    a. Entre las [ 6h y las 12h ): **Buenos días**.
    
    b. Entre las [ 12h y las 19h ): **Buenas tardes**. 
    
    c. Entre las [ 19h y las 0h ): **Buenas noches**.
    
    d. Entre las [ 0h y las 6h ): **Vete a acostarte**

33. **Crear-Usuario.ps1**: Pedir todos los datos del usuario por teclado y si no existe, crearlo (ver comando `New-LocalUser`). Los datos a solicitar son **nombre de usuario** (username), **contraseña** (password) y **nombre completo** (por ejemplo, "Perico de los Palotes").

34. **Eliminar-Usuario.ps1**: Mostrar un listado sólo con los nombres de todos los usuarios activos del sistema y al elegir uno que se elimine completamente del sistema. Debe pedir confirmación antes de eliminarlo.
