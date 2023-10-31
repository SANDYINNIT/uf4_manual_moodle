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
`(IP)/moodle/user/profile.php` - **Going into here**

![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Profile%20Settings.png)
Here you'll be able to change your username, password, etc.

## Change site name and settings.
To being able to change the site name you'll be able to do it in `Site Administration` site which is in `(IP)/moodle/admin/search.php`
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Site%20Administrator.png)

from there you should be able to see something saying `Site home settings` at the bottom, go there. `(IP)/moodle/admin/settings.php?section=frontpagesettings`
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Change%20Site%20name%20and%20settings.png)

And at the bottom you'll be able to change who's allowed to view front page, etc.
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Bottom%20to%20change%20settings%20of%20front%20page.png)

## Check if the time slot is correct.
To check that the time slot of your site is correct can be done by going to Site `Administration > Location > Settings.`

Basically `(IP)/moodle/admin/settings.php?section=locationsettings`
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Location%20Settings%20Innit.png)

You'll see this page *shown below,* and here It'll allow you to set a default timezone so you can correct it.. For Example mine isn't supposed to be `Europe/London` instead it's supposed to be `Europe/Madrid`... I'll change it.
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Timezone%20Change.png)

## Change the password policy.
We're gonna have to make it so that users who create themselves have a password of at least 4 characters including uppercase, lowercase and numbers. This can be done by going to Site `Administration > Security > Site Policies.`

Go here in `Site security settings`: `(IP)/moodle/admin/settings.php?section=sitepolicies`
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Security%20to%20change%20policy.png)

In the page you'll have to scroll a little down to being able to change it so the password length can be 4 characters and etc. *Shown below*
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Changing%20settings%20to%20able%20to%20put%201%20letter%20value%20in%20courses%20name.png)

## Create Courses
Now we'll have to create the following courses: a course named A (uncategorized) that consists of 3 topics and another named B (also uncategorized) that consists of 5 topics. We can do all this from Site `Administration->Manage courses and categories` or also from the Navigation box by going to `Courses > Add course`
*We'll be doing it from `Administration->Manage courses and categories`*
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Courses%20where.png)

In courses *shown from the screenshot above* go to `Manage courses and categories`
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Courses%20category.png)

After going to `(IP)/moodle/course/management.php` the page should look like the screenshot below.. BUT you won't have any courses,
To add courses click on `Create new course` at the right side and create 2 Courses called `A` and `B`, if it doesn't allow you to put short names then go to settings and disable it.
If you do not want the Category to be shown click on the eye button at the left side in the `Category` section, and it'll make the category be hidden.. Except for Administrators.
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Adding%20Courses.png)

## Edit Courses and add Topics, Materials, etc inside it.
Now we'll have to go to one of the courses created in the previous point (simply by selecting it in the Navigation box) and we are going to make it contain some type of material in one of its topics (a PDF document, for example),
Change the title of a topic and in General.

In `(IP)/moodle/course/` go to any Courses.. But I'm going to be Editing the Course `B`
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Editing%20where%20are%20courses.png)

In the Course, of course turn on `Editing mode` from top right.
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Editing%20mode%20turn%20on%20courses.png)

**Let's Edit This!:** *shown in the screenshot below*
We're gonna change `Topic 1` to `PDF Innit` and we'll also be adding an `PDF Recourse` in there. *dont forget to add a name to PDF while adding a file*
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Topic%20edit%20and%20adding%20PDF%20Course.png)

This is how it should look afterwards:
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/PDF%20and%20Topic%20change%20done.png)

## Creating an User
Let's manually create a user named **Bob** who must use the manual authentication method. This can be done from Site `Administration > Users > Accounts > Add User` aka `(IP)/moodle/admin/search.php#linkusers`

Once we're there we gotta create a new user by clicking on `Add a new user`:
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Adding%20User.png)

While creating a User, this is how it should look: *and simply save after done*
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Bob.png)

Done!:
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Users.png)

## Generating 10 Students
Let's generate ten students who will be from the two years A and B. we're going to use a CSV file to perform this block creation. Go to Site `Administration > Users > Accounts > Upload Users` and follow the prompts.

Once in `(IP)/moodle/admin/tool/uploaduser/index.php` we'll need to download the `example.csv` which is at the middle.
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Creating%2010%20Users%20CSV.png)

Open the file as an `LibreOffice Calc` and this is how it should look: 
*simply add usernames, firstnames, lastnames 7 more times.*
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/example-csv.png)

After you're done this is how it should look: 
*Simply now upload it to `Upload Users` and let it do its magic*
![Screenshot](https://github.com/SANDYINNIT/uf4_manual_moodle/blob/main/Screenshots/Done%20example-csv.png)