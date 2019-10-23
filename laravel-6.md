# Curso Laracast 
## Laravel 6 From Scratch 
### Section 1 - Prerequisites 

- At a glance 

La gestión principal de Laravel se realiza en fichero routes.php que define que controlador actua sobre la url solicitada.
Laravel realiza los siguientes pasos en cada llamada:
1. Procesa el fichero de rutas (Routes.php) para ver el controlador que actua sobre la url solicitada
2. El controlador (ej: PizzaController.php) solicita los datos al proveedor (ej: Pizza.php)
3. Con esos datos el controlador solicita la vista (pizza.blade.php), añade las dependencias necesarias (.js, .css, etc) y construye la visualización final

- Install PHP, MySQL and Composer

Instalación del entorno de desarrollo, dependencias e IDE

PHP >=7.2, MySQL y Composer

- The Laravel Installer

Revisar la documentacion oficial de laravel para instalar a traves de composer o añadiendo el comando laravel a PATH
Crear un projecto en blanco
```sh
# Instalar laravel
composer global require laravel/installer

# Comprobar la instalacion de laravel 
laravel 

# Si fuera necesario...
# Añadir al PATH (en Ubuntu 18.04)
export PATH="$HOME/.composer/vendor/bin:$PATH"

# Crear un proyecto en blanco
laravel new project-name
cd project-name
php artisan serve
# Abrir en el navegador http://localhost:8000

```

- Laravel Valet (Homestead) Setup

Revisar la documentacion oficial de laravel para instalar Valet (Homestead)

Instalar VirtualBox 6.x  y Vagrant 2.2.6

Instalar la Homestead Vagrant Box
```sh
vagrant box add laravel/homestead
```

Instalar Homestead
```sh
git clone https://github.com/laravel/homestead.git ~/Homestead
cd ~/Homestead
git checkout release

# Inciar configuracion (Creara el archivo Homestead.yaml)
# Mac / Linux o Consola Git de Windows...
bash init.sh
# Windows...
init.bat
```

Configurar Homestead (Homestead.yaml)
```sh
# Configurar el proveedor de maquina virtual 
# En el archivo Homestead.yaml (virtualbox, vmware_fusion, vmware_workstation, parallels or hyperv)
provider: virtualbox

# Configurar carpetas compartidas
folders:
	- map: ~/proyectos/project-name 
	  to: /home/vagrant/project-name
	 
# Configurar Nginx
# Si se cambia la propiedad sites se aprovisiona la Homestead Box
vagrant reload --provision
sites:
    - map: homestead.test
      to: /home/vagrant/project-name/public

# Añadir nombre de host
# C:\Windows\System32\drivers\etc\hosts
# /etc/hosts
192.168.10.10  homestead.test
```

Tips: Comandos vagrant 
```sh
vagrant up 
vagrant ssh
vagrant halt
vagrant reload --provision
vagrant destroy
vagrant box list
vagrant box prune
```



### Section 2 - Routing 

- Basic Routing and Views

/routes/web.php se definen las rutas del proyecto
/resources/view en este directorio se definen las vistas

- Pass Request Data to Views

Como pasar parametros por url a la vista

- Route Wildcards 

Como parametrizar las rutas y gestionar el contenido dependiendo del parametro

- Routing to Controllers

Gestionar las rutas a través de controladores y como crear controladores con php artisan

