# Curso Laracast

Acceso Laracast: 1nformatica@mkdrecambios.com[GXNqYb2PPL]

1. Básico 
  * [Laravel 6 From Scratch](https://laracasts.com/series/laravel-6-from-scratch)
  * [Simple Rules for Simpler Code](https://laracasts.com/series/simple-rules-for-simpler-code)
  * [Solid Principales](https://laracasts.com/series/solid-principles-in-php)

2. Intermedio
  * [Vue 2 step by step](https://laracasts.com/series/learn-vue-2-step-by-step)
  * [Be awesome in PHPStorm](https://laracasts.com/series/how-to-be-awesome-in-phpstorm) / [Visula Studio Code for PHP Developers](https://laracasts.com/series/visual-studio-code-for-php-developers)
  * [Testing Laravel](https://laracasts.com/series/phpunit-testing-in-laravel)

3. Avanzado 
  * [Build a Laravel app with TDD](https://laracasts.com/series/build-a-laravel-app-with-tdd)
  * [Testing Vue](https://laracasts.com/series/testing-vue)


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

/routes/web.php -> Fichero de definición de rutas 
/resources/view -> Directorio de vistas (welcome.blade.php / welcome.php)
```php
/* web.php */
Route::get( '/', function(){
	// Diferentes maneras de devolver contenido
	// Con una vista (welcome.blade.php)
	return view('welcome'); 
	// Texto literal
	return "Hello world!";
	// JSON
	return [ 'foo' => 'bar' ];
}
```

- Pass Request Data to Views

Podemos pasar parámetros a la vista a través de la URL

*laracast.test/?name=ruben*

```php
/* web.php */
Route::get( '/', function(){	
	$name = request('name');
	return view('welcome', [
		'name' => $name
	]);
});
```

```php
/* welcome.blade.php */
<html>
...
	<h1> {{ $name }} </h1> // con htmlspecialchars 
	<h1> {!! $name !!} </h1> // sin htmlspecialchars (Permite la inyección de codigo html o js)
```

- Route Wildcards 

Pasar por URL un parámetro comodín

*laracast.test/posts/first-post|second-post*

```php
/* web.php */
Route::get( '/posts/{post}', function($post){	
	// Datos de ejemplo a modo de base de datos
	$posts = [
		'first-post' => 'My first post',
		'second-post' => 'Second post'
	];
	
	if(! array_key_exist($post, $posts)){
		abort(404, 'Post not found.');
	}
	
	return view('post', [
		'post' => $posts[$post]
	]);
});
```

```php
/* post.blade.php */
<html>
...
	<h1> {{ $post }} </h1>  
```

- Routing to Controllers

Utilizando controladores para las rutas

*laracast.test/posts/first-post|second-post*

```sh
/* web.php */
Route::get( '/posts/{post}', 'PostController@show');
```

Para crear un controlador:
1. Nuevo archivo en /app/http/controllers
2. Con php artisan
```sh
php artisan make:controller PostController
php artisan // Para ver más opciones
```

```php
/* PostController.php */
namespace App\Http\Controllers;
use Illuminate\Http\Request;
class PostController extends Controller {
	public function show($post){
		$posts = [
		'first-post' => 'My first post',
		'second-post' => 'Second post'
	];
	if(! array_key_exist($post, $posts)){
		abort(404, 'Post not found.');
	}
	return view('post', [
		'post' => $posts[$post];
	]);
}
```

### Section 3 - Database Access

- Setup a Database Connection

En el archivo /.env configuramos la conexión a la base de datos

En /config/database.php se encuentran las diferentes configuraciones para las diferentes bases de datos

> ¡OJO! Ten en cuenta que si estas trabajando con Homestead, la base de datos local se encuentra en la máquina virtual y no en el equipo local, a la que se accedería si desplegaramos el proyecto con php artisan serve

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class PostController extends Controller
{
    public function show($slug)
    {
        $post = \DB::table('posts')->where('slug', $slug)->first();
	
	if(! post){
		abort(404);
	}

        return view('post', [
            'post' => $post
        ]);
    }
}
```

- Hello Eloquent

La clase Eloquent nos permite crear modelos que definen las tablas de la base de datos y nos ayudarán a crear el API, mantendremos el código más limpio y podremos definir reglas de negocio fácilmente.
```sh
// El nombre del modelo debe coincidir con el de la tabla de la base de datos
php artisan make:model Posts
```

Se creará el archivo para el modelo en /App/Http/Controllers/

Para utilizar los modelos, añadir "use App\Posts"


```php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

// Es necesario añadir la ruta al modelo
use App\Posts;

class Posts extends Controller
{
    public function show($slug)
    {
	// fistOrFail provoca una excepcion si no encuentra registros (abort(404))
	$post = Posts::where('slug', $slug)->firstOrFail();

        return view('post', [
            'post' => $post
        ]);
    }
}
``` 

- Migrations 101

Las migraciones nos ayudan a desplegar la base de dastos sin necesidad de SQL.

Para crear una migración, por ejemplo, para crear una tabla 
```sh
php artisan make:migration create_posts_table
```
Se creará el archivo correspondiente en App/Database/migrations donde podremos definir los campos de las tablas y sus propiedades.
```sh
php artisan migration // Para iniciar la migracion (crear tablas y demás)
php artisan migrate:rollback // Para retroceder a la última migración
php artisan migrate:fresh // Reconstruye la base de datos
```

- Generate Multiple Files in a Single Command

En un proyecto Laravel para definir objetos crearemos un modelo, un controlador y una migración, podemos realizar las tres acciones al mismo tiempo con el comando:
```sh
php artisan make:model Modelo -mc
```

- Business Logic

En Laravel disponemos de una consola que nos permite interactuar con la base de datos
```sh
php artisan tinker

// Guardar un registro en BBDD
$assignment = new App\Assignment;
$assignment->body = 'Finish school';
$assignment->save();

// Realizar consultas
App\Assignment::all();
App\Assignment::first();
App\Assignment::where('completed', false)->get();

// Podemos definir funciones en el modelo para ejecutar en la consola tinker
/*
// App\Http\Assignment.php
class Assignment extends Model
{
    public function complete(){
        $this->completed = true;
        $this->save();
    }
}
*/
$assignment = App\Assignment::first();
$assignment->complete();
```

### Section 4 - Views

- Layout Pages

Se pueden crear marcos que nos sirvan de plantillas para crear otras páginas
```php
/* layout.blade.php */
<html>
	<head></head>
	<body>
		@yield('content')
		<!-- @yield('content-2') -->
		<!-- @yield('content-3') -->
	</body>
</html>
	
/* wellcome.blade.php */

@extends('layout')
@section('content')
            <div class="content">
                <h6>wellcome.blade</h6>
            </div>
@endsection
	
```

- Integrate a Site Simple

En el directorio /public se pueden agregar librerias externas, se pueden añadir directorios accesibles /css, /js...

- Set an Active Menu Link

Para marcar los botones de navegación activos en cada URL podemos hacerlo de una manera sencilla con los atributos de Request:
```php
<ul>
	<li class="{{ Request::path() == '/' ? 'current_page' : '' }}"><a href="/">Home</li>
	<li class="{{ Request::path() == '/about' ? 'current_page' : '' }}"><a href="/about">About</li>
	<li class="{{ Request::path() == '/contact' ? 'current_page' : '' }}"><a href="/contact">Contact</li>
</ul>
```

- Asset Compilation with Laravel Mix and Webpack

Laravel Mix nos proporciona una manera sencilla de trabajar con pre-procesadores de CSS y JS

Necesitaremos Nodejs y NPM
```sh
node -v
npm -v
```

En webpack.json definimos los ficheros de entrada y salida
```js
mix.js('resources/js/app.js', 'public/js')
    .sass('resources/sass/app.scss', 'public/css');
```

En package.json definiremos las dependencias de nuestro proyecto, debemos instalar estas dependencias
```sh
npm install
```

Laravel Mix es solo la capa superior gestionada por Webpack, en package.json también encontramos los comando que necesitaremos ejecutar para realizar la copilación de los ficheros
```sh
npm run dev // Compila nuestros archivos 
npm run watch // Compila los archivos en vivo 
npm run production // Compila los archivos en versión minificada
```

Laravel Mix trabaja con los principales pre-procesadores de CSS y JS: React, LESS, SASS, Stylus, PostCSS... 

Se suele asociar un tiemstamp a la ruta de acceso a los archivos compilados para forzar a los navegadores a recargar el archivo, Laravel Mix puede manejar esto con el metodo version()
```js
mix.js('resources/js/app.js', 'public/js').version();
```
Tras generar el archivo compilado, tendrá el mismo nombre, para gestionar esto en nuestro HTML realizamos el enlace al archvivo de la siguiente manera: 
```html
<script src="{{ mix('/js/app.js') }}"></script>
```
Está práctica es utilizada en los entornos de producción, para realizar este filtro en nuestro webpack.json 
```js
mix.js('resources/js/app.js', 'public/js');
if (mix.inProduction()) {
    mix.version();
}
```

- Render Dinamyc Data
- Render Dinamyc Data: Part 2
- Homework Solutions


