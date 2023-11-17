# SSH

**SSH** (o **S**ecure **SH**ell) es un protocolo para acceder a un servidor por medio de un canal seguro en el que toda la información está cifrada.

También permite copiar datos de forma segura (SFTP), gestionar claves RSA para no utilizar contraseñas al conectar a los dispositivos y crear túneles seguros para el intercambio de datos de cualquier otra aplicación. 

El puerto TCP asignado por defecto es el 22.

Está diseñado para reemplazar los métodos antiguos y menos seguros para conectarse remotamente a otro sistema a través de un intérprete de comandos, ya que a diferencia de otros protocolos de comunicación remota tales como FTP o Telnet, SSH encripta la sesión de conexión, haciendo imposible que alguien pueda obtener contraseñas no encriptadas.

## OpenSSH

[**OpenSSH**](https://www.openssh.com/) (Open Secure Shell) es la alternativa libre y abierta al programa **Secure Shell**, que es software propietario, y se ha convertido en el estándar SSH. Existen implementaciones de OpenSSH para GNU/Linux, Windows y Mac OS.

## Instalación y configuración de OpenSSH

- Windows

- GNU/Linux

Pasos para instalar OpenSSH.

### GNU/Linux

Las herramientas cliente de OpenSSH normalmente vienen instaladas en los sistemas GNU/Linux. Lo que se explica a continuación es como instalar el servicio OpenSSH.

#### Distribuciones basadas en Debian

```bash
sudo apt install -y openssh-server
```

### Windows

OpenSSH se encuentra disponible como una característica desde Windows 10 y Windows Server 2019. Para versiones anteriores consultar la [version portable](https://github.com/PowerShell/OpenSSH-Portable).

Para instalarlo debemos ir a **Configuración > Aplicaciones > Características opcionales**:

![](assets/2022-11-24-17-10-48-image.png)

A continuación pulsamos el botón **Ver características** e introducimos en el cuadro de búsqueda `OpenSSH`:

![](assets/2022-11-24-17-10-06-image.png)

Seleccionamos el cliente y/o el servidor, pulsamos **Siguiente** y luego **Instalar**. Cuando el proceso termine y ya tendremos instaladas las características seleccionadas.

El servidor OpenSSH se debe configurar como un servicio de Windows y/o iniciar manualmente, siguiendo los siguientes pasos:

```powershell
# Iniciar el servicio sshd
Start-Service sshd

# Hacer que el servicio inicie automáticamente (opcional pero recomendable)
Set-Service -Name sshd -StartupType 'Automatic'

# Confirmar que hay una regla en el Firewall que permite las conexiones SSH
# (en principio, al instalar el servicio se configura automáticamente esta regla)
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```

## Autenticación basada en contraseñas



## Autenticación basada en certificados
