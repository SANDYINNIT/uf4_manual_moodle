# Instaling Moodle Web

## Vagrant
We have to install `Vagrant`, with Vagrant we'll be able to have a MV to do this read below.
```console
smx2b@turing-116:~$ mkdir Ejemplo
smx2b@turing-116:~$ cd Ejemplo
```

We can directly create `Vagrantfile` with the next command `vagrant init <box>`.
```console
smx2b@turing-116:~$ vagrant init ubuntu/jammy64
smx2b@turing-116:~$ ll
```

After installing an MV into the folder, use this command next to turn it on `vagrant up` `--provider=virtualbox`.
```console
smx2b@turing-116:~$ vagrant up --provider=virtualbox
```
After getting it up do `ssh`, To enter the MV. Afterwards try `ll` command to see where you're at.
```console
smx2b@turing-116:~$ vagrant ssh
smx2b@turing-116:~$ ll /vagrant
```
### Other Commands:
- `vagrant status`: Nos muestra informacion sobre el estado de las MVs.
- `vagrant halt`: Parar las MVs
- `vagrant suspend`: Suspender las MVs
- `vagrant up`: Encender las MVs
- `vagrant destroy`: Borrar las MVs

## Instalacion y configuracion de aplicaciones web
Para instalar una aplicacion web, necesitamos su codigo fuente y traerlo al directorio de nuestro servidor de aplicaciones, en este caso apache2. Cuanto instalas apache2 se crea una carpeta en `/var/www/html`. Si llevamos nuestra aplicacion al directorio `/var/www/html` tendremos acceso a nuestra aplicacion con la direccion `http://localhost`.

### Installing Apache2, Mysql, etc
- Firstly update the MV: `apt update` and `apt upgrade`.
```console
smx2b@turing-116:~$ apt update
smx2b@turing-116:~$ apt upgrade
```
1. Installing `apache2`.
```console
smx2b@turing-116:~$ apt install -y apache2
```
2. Installing data base server `mysql-server`.
```console
smx2b@turing-116:~$ apt install -y mysql-server
```
3. Installing `php` (Lenguaje principal que usan las aplciaciones).
```console
smx2b@turing-116:~$ apt install -y php libapache2-mod-php
smx2b@turing-116:~$ apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl
```
4. Restart apache2
```console
smx2b@turing-116:~$ systemctl restart apache2
```
- RECORDAR SER ROOT

### Configuracion MySQL
#### Let's go into mysql
Before doing this be in `root`.
```console
smx2b@turing-116:~$ mysql
```
#### Creation of BBDD
Inside mysql, use the following command
```console
CREATE DATABASE bbdd;
```
Este comando nos creara una base de datos con el nombre `bbdd`.
#### Creation of an user
```console
CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
#### We'll assign permissions to the user and then exit.
```console
GRANT ALL ON bbdd.* to 'usuario'@'localhost';
```
```console
exit
```
#### Move zip file to the system (Moodle)
Get the zip file required to being able to install moodle server, and then move it into the same category where you had previously made the Vagrant file inside. Afterwards use the following command:
```console
smx2b@turing-116:~$ mv /vagrant/moddle.zip /var/www/html/
```
#### Export de .zip
Firstly do `sudo apt install zip` before doing the following command.
```console
smx2b@turing-116:~$ unzip moodle.zip
```

#### Give permissions
```console
cd /var/www/
chmod -R 775 .
chown -R root:www-data .
```

#### Give permission
```console
cd /var/www/html
chmod -R 775 .
chown -R root:www-data .
```
## Activar visibilidad de la web
Para hacer visible la web, tendremos que modificar algunos parametros en el archivo `vagrantfile`.
```console
vi Vagrantfile
```
```console
# Create a forwarded port mapping which allows access to a specific port
# within the machine from a port on the host machine. In the example below,
# accessing "localhost:8080" will access port 80 on the guest machine.
# NOTE: This will enable public access to the opened port
config.vm.network "forwarded_port", guest: 80, host: 8080
```
```console
# Create a public network, which generally matched to bridged network.
# Bridged networks make the machine appear as another physical device on
# your network.
config.vm.network "public_network"
```
#### Editar archivo `php`
```console
smx2b@turing-116:~$ vi /etc/php/8.1/apache2/php.ini
```
Añadimos esta linia al archivo `max_input_vars = 5000

 # Assignment to do inside Moodle Website

 ## Change Profile, name, etc.
 To being able to do this go to your `Profile` *at the top right* by left clicking on your profile.

![](../../../Imatges/Captures%20de%20pantalla/a.png)