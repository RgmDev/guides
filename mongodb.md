# MongoDB

## Consultas básicas 


```
# Mostrar bases de datos
show dbs

# Crear y usar una base de datos
use curso_node_angular

# Añadir un registro a un fichero de mongo 
db.bookmarks.save({id: 1, tittle: 'titulo', foo: 'bar'})

# Mostrar el contenido de un fichero de mongo
db.bookmarks.find()
```

```
# Crear la base de datos
use music_app

# Insertar un registro en la tabla de artistas
db.artists.save({ name: 'Delafuente', description: 'Musica urbana', imagen: null });

# Mostrar el contenido de la tabla de artistas
db.artists.find();

# Buscar un _id en la tabla de artistas
db.artists.find({"_id": ObjectId("5f3c492349033231787cc8bd")});

# Eliminar un objeto de una coleccion por su _id
db.artists.remove({"_id": ObjectId("5f691dd01f4c921430d386af")});
```
