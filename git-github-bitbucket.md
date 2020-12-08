# Configurar git desde cero

## Escenario 

Tenemos nuestra cuenta de github y un repositorio local para agregar a nuestra cuenta 

## Variables de configuración 

Configurar el nombre de usuario y el correo que identificara los commits
```sh
git config --list
git config --global user.name "rugo"
git config --global user.email rubengm410@gmail.com
```

## Configurar las claves SSH

Generamos la clave SSH que identifica al equipo y la añadimos al SSH Agent
```sh
ssh-keygen -t rsa -b 4096 -C "rubengm410@gmail.com"
eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_rsa
```
En nuestra cuenta de Github >> **Settings** >> **SSH and GPG keys** >> **New SSH Key**

Agregar el contenido de **~/.ssh/id_rsa.pub** y un título descriptivo

## Iniciar el repositorio en el directorio local, realizar el primer commit y guardar los cambios en el repositorio remoto
```sh
git init
git add .
git commit -m "commit inicial"
git push -u origin master
```



# Git

## Sincronizar las claves SSH 
https://medium.com/@ancizj393/crear-una-clave-ssh-en-git-y-vincular-en-tu-cuenta-de-github-e7a6b22bc93f

## Fix Error
Permission denied (publickey)

https://stackoverflow.com/questions/16074832/cannot-push-to-git-repository-on-bitbucket/28034014

## Primeros pasos, ejemplo simple
https://codigofacilito.com/articulos/commits-administrar-tu-repositorio
https://stackoverflow.com/questions/24114676/git-error-failed-to-push-some-refs-to

Tenemos un proyecto local y queremos repositarlo y gestionarlo con git/Github 

1. Creamos el repositorio en nuestra cuenta de Github

2. Iniciamos el control del proyecto con git en nuestra máquina local
  git init
  
3. Guardamos los cambios en el repositorio local 
  git add .
  git status
  git commit -m "Comentario"
  
4. Añadimos el origen de nuestro repositorio
  Copiamos la url que nos asigna github a nuestro repositorio (ej: https://github.com/Usuario/repositorio.git)
  git remote add origin https://github.com/Usuario/repositorio.git
  
5. Subimos los archivos
  git push origin master
