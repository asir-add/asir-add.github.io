# Configurar un servicio SystemD en GNU/Linux

Se va a configurar el binario/script "/path/to/FOO" como un servicio, de forma que inicie automáticamente en el nivel de ejecución 5, que en nuestro caso es el nivel de ejecución por defecto en LinuxMint 18.2. 

**NOTA: "FOO" puede estar en cualquier otra ubicación, y debe tener permisos de ejecución.**

**OJO: Reemplazar "/path/to/FOO" con la ruta al binario del servicio a iniciar.** 

1.  **Abrir una BASH como "root":**

```bash
$ sudo bash
```

2.  **Crear el "unit file" del servicio "foo" en  /etc/systemd/system:**

    Creamos el fichero /etc/systemd/system/foo.service:

```bash
[Unit]
Description=Servicio FOO
After=syslog.socket

[Service]
KillMode=process
ExecStart=/bin/bash -c /path/to/FOO
Restart=always

[Install]
WantedBy=runlevel5.target
```

Descripción de cada una de las propiedades usadas en el fichero anterior:

| [UNIT]      |                                          |
| ----------- | ---------------------------------------- |
| Description | Texto descriptivo del servicio.          |
| After       | Indicamos después de que servicios/targets/sockets se inicia el nuestro. En nuestro caso es después de iniciar el socket “syslog” pues nuestro servicio usa el log del sistema. |

| [SERVICE] |                                          |
| --------- | ---------------------------------------- |
| KillMode  | Indicamos la forma en la que se detiene el servicio; en este caso, “process” indica que se enviará la señal sólo al proceso principal. Éste deberá ocuparse de todos sus hijos. |
| ExecStart | Indicamos el comando que inicia el servicio. *NOTA: En nuestro caso, al tratarse de un script, lo que ejecutamos es una BASH que a su vez ejecutará el script FOO.* |
| Restart   | En caso de que el servicio falle o se mate al proceso y el servicio se detenga, con “always” se reiniciará automáticamente. |

| [INSTALL] |                                          |
| --------- | ---------------------------------------- |
| WantedBy  | Indicamos los targets que dispararán la ejecución de nuestro servicio. En nuestro caso usamos “runlevel5.target” que es el equivalente a iniciar en el nivel de ejecución 5.  Otros valores que suelen usarse son “multi-user.target” y “graphical.target”. |

3.  **Avisamos a SystemD que cargue de nuevo los “unit files” incluido el nuestro:**

```bash
# systemctl daemon-reload
```

Cada vez que hagamos un cambio en un “unit file” es necesario ejecutar este comando para que SystemD vuelva a cargarlo.

4.  **Verificamos el funcionamiento del servicio:**

   Comprobamos si se inicia y detiene “/path/to/FOO” correctamente:

```bash
# systemctl status foo
● foo.service - Servicio FOO
Loaded: loaded (/etc/systemd/system/foo.service; disabled; vendor …)
Active: inactive (dead)
# systemctl start foo
# systemctl status foo
● foo.service - Servicio FOO
Loaded: loaded (/etc/systemd/system/foo.service; disabled; vendor…)
Active: active (running) since dom 2017-10-08 12:36:07 WEST; 23s ago
Main PID: 3744 (FOO)
CGroup: /system.slice/foo.service
		├─3744 /bin/bash /path/to/FOO
		└─3754 sleep 10s

oct 08 12:36:07 fran-VirtualBox systemd[1]: Started Servicio FOO.
```

Comprobamos que existe el proceso FOO

```bash
# ps -e | grep FOO
 3744 ?        00:00:00 FOO
# systemctl stop foo
# systemctl status foo
● foo.service - Servicio FOO
   Loaded: loaded (/etc/systemd/system/foo.service; disabled; vendor...)
   Active: inactive (dead) since dom 2017-10-08 12:37:37 WEST; 1s ago
  Process: 3744 ExecStart=/bin/bash -c /path/to/FOO (code=killed, signal=TERM)
 Main PID: 3744 (code=killed, signal=TERM)
   CGroup: /system.slice/foo.service
           └─3785 sleep 10s
[...]
oct 08 12:37:37 fran-VirtualBox systemd[1]: Stopping Servicio FOO...
oct 08 12:37:37 fran-VirtualBox systemd[1]: Stopped Servicio FOO.
```
El servicio tarda un poco en detenerse, por eso a los pocos segundos volvemos a ejecutar un “status” indicando que los procesos del servicio están muertos.
```bash
# systemctl status foo
● foo.service - Servicio FOO
   Loaded: loaded (/etc/systemd/system/foo.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
[...]
```
Ahora comprobamos que el servicio reinicia si matamos el proceso principal.
```bash
# systemctl start foo
# ps -e | grep FOO
3898 ?        00:00:00 FOO
# killall FOO
# ps -e | grep FOO
4005 ?        00:00:00 FOO
```
Se observa que por mucho que matemos al proceso “FOO”, SystemD vuelve a iniciarlo (directiva Restart).

5. **Habilitamos el servicio:**

Es necesario habilitar el servicio para que se inicie automáticamente al iniciar el sistema:

```bash
# systemctl enable foo
```

Esto creará el enlace simbólico “/etc/systemd/system/runlevel5.target.wants/foo.service” apuntando al “unit file” de nuestro servicio (“/etc/systemd/system/foo.service”).  

Para verificar que nuestro servicio se inicia automáticamente, sólo tendremos que reiniciar el equipo con “reboot” y verificar con el comando “ps” que existe el proceso “FOO” correspondiente.

En caso de que no quisiéramos que se iniciase automáticamente sino de forma manual, debemos deshabilitarlo:

```bash
# systemctl disable foo
```

Eliminará el enlace simbólico creado antes.