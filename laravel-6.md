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
Instalación del entorno de desarrollo,dependencias e IDE de desarrollo
PHP 7.2.8
MySQL 
Se puede utilizar XAMPP para PHP y MySQL 
Composer 

- The Laravel Installer
Revisar la documentacion oficial de laravel para instalar a traves de composer o añadiendo el comando laravel a PATH
Crear un projecto en blanco
```sh
# Instalar laravel
composer global require laravel/installer
# Añadir al PATH, en Ubuntu 18.04
export PATH="$HOME/.composer/vendor/bin:$PATH"
# Crear un proyecto en blanco
laravel new project-name
```

- Laravel Valet Setup
Revisar la documentacion oficial de laravel para instalar Valet (Homestead)
```sh
# Instalar Nirtual box de la pagina oficial (https://www.virtualbox.org/wiki/Linux_Downloads)
vboxmanage --version
#Instalar vagrant, descargar el .deb de la pagina oficial
sudo dpkg -i vagrant_2.2.6_x86_64.deb
vagrant --version

# Añadir homestead box
vagrant box add laravel/homestead

# Clonar el repositorio y activar la rama con la ultima versión estable del proyecto
git clone https://github.com/laravel/homestead.git ~/Homestead
cd ~/Homestead
git checkout release
#Iniciar el proyecto (Se crea el archivo Homestead.yaml)
cd ~/Homestead
bash init.sh # en WIndows init.bat

# Configurar el archivo Homestead.yaml (si se cambia este archivo ejecutar: vagrant reload --provision)
folders:
	- map: ~/proyectos/project-name 
	  to: /home/vagrant/project-name

sites:
    - map: homestead.test
      to: /home/vagrant/project-name/public

``` 


### Section 2 - Routing 

- Basic Routing and Views
/routes/web.php se definen las rutas del proyecto
/resources/view/welcome.blade.php en este directorio se definen las vistas

- Pass Request Data to Views
Como pasar parametros por url a la vista

- Route Wildcards 
Como parametrizar las rutas y gestionar el contenido dependiendo del parametro

- Routing to Controllers
Gestionar las rutas a través de controladores y como crear controladores con php artisan

