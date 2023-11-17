# Instalación y configuración de OpenSSH en GNU/Linux

Las herramientas cliente de OpenSSH normalmente vienen instaladas en los sistemas GNU/Linux. Lo que se explica a continuación es como instalar el servicio OpenSSH.

## Instalación

### Distribuciones basadas en Debian

```bash
sudo apt install -y openssh-server
```

## Configuración

SSH ofrece múltiples opciones de autenticación: contraseñas, claves públicas y certificados.

### Autenticación basada en contraseñas

Este método de autenticación del servicio SSH es el que suele venir configurado por defecto. Para deshabilitarlo debemos modificar el siguiente parámetro en el fichero de  configuración del servicio `/etc/ssh/sshd_config`:

```ini
PasswordAuthentication yes
```

Si por el contrario queremos deshabilitar este método de autenticación:

```ini
PasswordAuthentication no
```

> Éste método es poco seguro, por lo que es recomendable utilizar una contraseña bastante fuerte.

### Autenticación basada en claves públicas

Desde el equipo cliente, el que quiere conectarse al servidor SSH, debemos generar un par de claves (pública y privada):

```bash
> ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\fvarrui/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Passphrases do not match.  Try again.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\fvarrui/.ssh/id_rsa
Your public key has been saved in C:\Users\fvarrui/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:iamCvLNrViZIcfUx9SYPx2MsCi2PSgD/9JuV+5taVDQ fvarrui@CENTOLLO
The key's randomart image is:
+---[RSA 3072]----+
|.   .. o..  E    |
|.o .  o o +. .   |
| .+ .o o + O.    |
| ..o .=o.oO..    |
|o  ...+oS ..     |
|oo.o.. + o       |
|..=.. o . .      |
| +..     o .     |
|o++     ..+.     |
+----[SHA256]-----+
```

### Autenticación basada en certificados

Éste es el método de autenticación más seguro de los que ofrece SSH.

[TODO]

## Referencias

- [How to configure SSH Certificate-Based Authentication](https://goteleport.com/blog/how-to-configure-ssh-certificate-based-authentication/)

- [How to configure and setup SSH certificates for SSH authentication - DEV Community 👩‍💻👨‍💻](https://dev.to/gvelrajan/how-to-configure-and-setup-ssh-certificates-for-ssh-authentication-b52)

- [OpenSSH Config File Examples For Linux / Unix Users - nixCraft](https://www.cyberciti.biz/faq/create-ssh-config-file-on-linux-unix/)

- [How to Manage an SSH Config File in Windows and Linux](https://www.howtogeek.com/devops/how-to-manage-an-ssh-config-file-in-windows-linux/)
