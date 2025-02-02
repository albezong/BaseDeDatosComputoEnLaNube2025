# CRUD Y CONSULTAS EN Mongodb

### Crear una base de datos
Solo se crea si contiene por lo menos una coneccion

### Como utilizar show
Para conocer todas las bases de datos 
del servidor de MongoDB utilizamos el comando:
``` json
show dbs
```

### Como utilizar "use" para utilizar una coleccion "db1"
Mediante el comando use activamos la 
base de datos que queremos trabajar:
``` json
test> use bdejemplo
switched to db bdejemplo
bdejemplo>
```

## Como utilizar "show collections"
Mostrar colecciones en la base de datos actual
``` json
> show collections
coleccion1
coleccion2
```
## Como crear una coleccion
para crear una nueva colección.
``` json
db.createCollection("nombre_de_coleccion")
```

## Borrar coleccion 
``` json
db.dropDatabase()
```

## Mostrar las colecciones de esa bd 
``` json
show collections
```

## Insertar un solo documento en una colección
El método **insertOne()** en MongoDB se utiliza para 
insertar un solo documento en una colección, 
asignando automáticamente un único _id si no se 
proporciona, y admitiendo preocupaciones de escritura 
y transacciones de varios documentos.

``` json
bdejemplo> db.alumnos.insertOne( { 
    nombre: 'Soyla',
    apellido1: 'Vaca',
    edad: 32,
    ciudad: 'San Miguel de las piedras'
}
)
```

## Inserccion de un documento mas complejo con arrays

```json
bdejemplo> db.alumnos.insertOne(
{
    nombre: "Joaquin",
    apellido1: "Dorian",
    apellido2: "Guerrero",
    edad: 15,
        aficiones:[
            'Cerveza', 'Hueva', 'Canabis'
        ]
    }
)
```

## Select * from alumnos
``` json
db.alumnos.find({})
```

## Insersion de documentos mas complejos con documentos anidados y ID

```json
db.alumnos.insertOne(
{
    nombre: "Jose Luis",
    apellido1: "Herrera",
    apellido2: "Gallardo",
    edad: 41,
    estudios: [
            'Ing en Sistemas Computacionales', 
            'Maestria en Tecnologias de la Informacion'
        ],
    experiencia: {
        lenguaje: "SQL",
        sbd: "SQL Server",
        aniosExperiencia: 14
        }
    }
)


db.alumnos.insertOne({
    _id: 3,
    nombre:"Sergio",
    apellido:"Ramos",
    equipo: "Gallardios",
    aficiones: ["dinero","hombres","Fiesta"]
    talentos: {
        futbol: true, 
        bañarse: false
    }
}
)

```

## Consulta de igualdad
``` json
db.alumnos.find( { _id: 5 } )
```

## Insertar multiples documentos

```json
db.alumnos.insertMany({
    [
        {
            _id: 12,
            nombre: 'Roberto',
            apellido: 'Gomez',
            edad: "23",
            descripcion: "Es un comediante bueno"

        },
        {
            nombre: 'Luis',
            apellido: "Suarez",
            edad: 43,
            habilidades : [
                'Correr', 'Dormir', 'Morder'
            ],
            direcciones: {
                calle: 'Del infierno',
                numero : 666
            },
            esposas:[
                {
                    nombre: 'Maria', 
                    edad: 20,
                    pension: 350,
                    hijos: ['Joaquin', 'Bridget']
                    },
                    {
                        nombre: "Dorian",
                        edad: "46",
                        pension: 4500.56,
                        complaciente: true
                    }
            ]
        }
    ]
}
)
```

# Practica de --libros--
1. Bases de datos, colecciones e inserts

    . Nos conectamos con mongosh al mongsh
    . 


    ```json
    use curso
    ```

    . Verificar que la base de datos no existe

    show bds

     Crear una coleccion denominada "facturar"