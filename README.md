### Configuración de Red y Preparación del Sistema

#### Ver configuración de red actual:
```bash
ip a
```

#### Editar la configuración de red:
```bash
sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3
```

Agregar o modificar las siguientes líneas en el archivo:

```ini
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no
IPV6_AUTOCONF=no
IPV6_DEFROUTE=no
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=2e236d2b-1dfe-43a4-be47-636ffa05d85c
DEVICE=enp0s3
ONBOOT=yes
IPADDR=192.168.1.110
PREFIX=24
GATEWAY=192.168.1.1
DNS1=192.168.1.100
PEERDNS=no
```

#### Reiniciar la red:
```bash
sudo systemctl restart network
```

#### Cambiar nombre de host
```bash
sudo nano /etc/hostname
sudo nano /etc/hosts
sudo reboot
```


### Instalación de Docker en CentOS

#### Instalar dependencias:
```bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

#### Agregar repositorio de Docker:
```bash
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

#### Instalar Docker:
```bash
sudo yum install -y docker-ce
```

#### Iniciar y habilitar Docker:
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

#### Agregar el usuario actual al grupo docker:
```bash
sudo usermod -aG docker $(whoami)
```

### Instalación de Docker Compose

#### Descargar Docker Compose:
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

#### Asignar permisos de ejecución:
```bash
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

#### Crear un enlace simbólico en `/usr/bin`:
```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
ls -l /usr/bin/docker-compose
```

### Desactivar SELinux

#### Editar configuración de SELinux:
```bash
sudo nano /etc/selinux/config
```

Cambiar la línea:
```ini
SELINUX=permissive
```

#### Reiniciar el sistema:
```bash
sudo reboot
```

#### Verificar estado de SELinux:
```bash
sestatus
```

### Instalación de Git

#### Instalar Git:
```bash
sudo yum install -y git
```

### Clonar Repositorio de Monitoreo

#### Clonar el repositorio:
```bash
git clone https://github.com/diegovega223/monitoreo.git
```

#### Cambiar permisos del directorio:
```bash
sudo chmod 777 -R monitoreo
cd monitoreo
```

#### Iniciar Docker Compose:
```bash
sudo docker-compose up -d
```

#### Cambiar permisos de los directorios:
```bash
cd ..
chmod 777 -R monitoreo
cd monitoreo
```

### Actualizar el sistema

#### Actualizar todos los paquetes:
```bash
sudo yum update -y
```

### Agregar al Dominio

#### Instalar paquetes necesarios:
```bash
sudo yum install -y realmd sssd adcli samba-common samba-common-tools oddjob oddjob-mkhomedir krb5-workstation
```

#### Descubrir el dominio:
```bash
sudo realm discover Blitzcode.company
```

#### Unirse al dominio:
```bash
sudo realm join --user=administrador Blitzcode.company
```

#### Ajustar permisos del archivo de configuración de sssd:
```bash
sudo chmod 600 /etc/sssd/sssd.conf
```

#### Habilitar y arrancar sssd:
```bash
sudo systemctl enable sssd
sudo systemctl start sssd
```

#### Habilitar y arrancar oddjobd:
```bash
sudo systemctl enable oddjobd
sudo systemctl start oddjobd
```

#### Configurar pam_mkhomedir:
```bash
sudo nano /etc/pam.d/common-session
```

Agregar la siguiente línea:
```ini
session    required     pam_mkhomedir.so skel=/etc/skel/ umask=0077
```

#### Reiniciar sssd:
```bash
sudo systemctl restart sssd
```

#### Verificar configuración de dominio:
```bash
realm list
ping Blitzcode.company
```

#### Acceder a usuarios de AD:
```bash
su - dvega@Blitzcode.company
su - msosa@Blitzcode.company
su - kvidir@Blitzcode.company
```

Contraseña de Active Directory: `User123`.

