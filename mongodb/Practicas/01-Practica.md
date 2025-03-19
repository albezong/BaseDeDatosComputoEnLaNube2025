# PRACTICA 1: BASE DE DATOS , COLECCIONES E INSERTS

- 1. Conectamos con mongosh a mongosh

- 2. Crear una base de datos llamada "curso"
``` json
use.curso
```

- 3. Comprobar que la base de datos no existe
``` json
show dbs
```

- 4. Crear una Coleccion que se llame facturas y se muestre
``` json
db.createCollection('facturas')
show collections
```
- 5. Insertar un documento con los siguientes datos:
| Codigo      | Valor        |
|-------------|--------------|
| Cod_Factura |    10        |
| Ciente      |Frutas Ramirez|
| Total       |    223       |

| Codigo      |   Valor      |
|-------------|--------------|
| Cod_Factura |     20       |
| Ciente      |Ferreteria Juan|
| Total       |      140     |


``` json
db.facturas.insertOne( { 
    cod-factura: 10,
    Cliente: 'Frutas Ramirez',
    total: 223
}
)

db.facturas.insertOne({
    cod_factura: 20,
    cliente: 'Ferreteria Juan',
    total: 140 
    })
```

- 6.Crear una nueva coleccion pero usando directamente el insertOne. Insertar un documento en la coleccioon
| Codigo      | Valor        |
|-------------|--------------|
| Cod_produto |    1        |
| Nombre      |Tornillo x 1"|
| Precio      |    2       |

``` json
db.producto
```

- 7. Crear un nuevo documento de producto que contenga un array. Con los siguientes datos:

| Codigo      | Valor       |
|-------------|-------------|
| Cod_produto |    2        |
| Nombre      |  Martillo   |
| Precio      |     500      |
| Unidades    |     100      |
| Fabricantes  | fac1,fac2, fac3 |

``` json
db.productos.insertOne({
    cod_produto: 2,
    nombre: "Martillo",
    precio: 20,
    unidades: 50,
    Fabricantes: ['fab1', 'fab2', 'fab3']
})
```

- 8. Borrar la colección de facturas y comprobar que se borro

``` json
show collections
db.facturas.drop()  # Borrar la colección
show collections
```

- 9. Insertar un documento en una coleccion denominda **fabricantes**. Para probar los subdocumentos y la clave _id personalizada

| Codigo      | Valor       |
|-------------|-------------|
|     id      |    1        |
| Nombre      |  fab1       |
| Localidad   |ciudad: buenos aires, pais: argentina, Calle: Calle pez 27, cod_postal:2900|

``` json
db.fabricantes.insertOne({
    _id: 1,
    nombre: "fab1",
    localidad: {
        ciudad: "buenos aires",
        pais: "argentina",
        calle: "calle pez 27",
        cod_postal: 2900
    }
})
```

- 10. Realizar una insersion de varios documentos ##(arreglo de documentos es un insertMany) ##
| Codigo      | Valor       |
|-------------|-------------|
| Cod_produto |    3        |
| Nombre      |  Alicates   |
| Precio      |     10      |
| Unidades    |     25      |
| Fabricantes  | fac1,fac2, fac5 |
--------------------------------------------
| Codigo      | Valor       |
|-------------|-------------|
| Cod_produto |    4        |
| Nombre      |  Arandelas   |
| Precio      |     1     |
| Unidades    |     500      |
| Fabricantes  | fac1,fac2, fac5 |

``` json
db.alumnos.insertMany({
    [
        {
            cod_produto: 3,
            nombre: "Alicates",
            precio: 10,
            unidades: 25,
            fabricantes: ["fac1", "fac2", "fac5"]
        },
        {
            cod_produto: 4,
            nombre: "Arandelas",
            precio: 1,
            unidades: 500,
            fabricantes: ["fac1", "fac2", "fac5"]
        }
    ]
    })
```




# CLASE DEL DIA 10/02/2025

# Practica1
## Operadores Lógicos
### Operador AND
#### Ejercicios

2. Mostrar todos aquellos libros que cuesten mas de 25 
y cuya cantidad sea inferior a 15 y id igual 4


``` json
show dbs
show collections
db.libros.find({
    $or:[{precion: { $gt: 25}},{cantidad: { $lt:15}}]
})
```

### AND y OR Combinadas
1. Mostrar los libros de la editorial biblio con precio mayor a 40 o libros de la editorial Planeta
con precio mayor a 30

``` json
db.libros.find(
{
    $or: [
        { $and:[{editorial: 'biblio'}, {precio: :{$gt:30}}]},
        { $and:[{editorial: {$eq:'Planeta'}}, {precio: :{$gt:20}}]}
    ]
}
)
```
#### forma simple
``` json
db.libros.find(
{
    $or: [
        {editorial: 'biblio'}, {precio: :{$gt:30}},
        {editorial: {$eq:'Planeta'}, precio: :{$gt:20}}
    ]
}
)
```

## Proyeccion de Columnas

*** Sintaxis ***
db.coleccion.find(filtro, columnas)
db.libros.find({},{titulo:1})

1. Seleccionar todos los documentos, mostrando el titulo y la editorial
``` json
db.libros.find({},{titulo:1, editorial:1, _id:0})
```

2. Seleccionar todos los documentos de la editorial planeta, 
mostrando solamente el titulo y la editorial
``` json
db.libros.find({editorial:'Planeta'}, {_id:0, titulo:1, editorial:1})
```

3. 



# ACTIVIDAD DEL DIA 13 03
git status
git add
//comit and y or combinaciones
Dia 13/02/2025

# OPERADOR exist (Permite saber si un campo se encuentra o no en un documento)

``` json
db.libros.find({
    editorial:{$exists: true}
    })
```

insertar un documento 


``` json
db.libros.insertOne({
    _id: 10,
    titulo: 'Mongo en entornos graficos',
    editorial: 'Terra',
    precio: 125
})
```

buscar
``` json
db.libros.find({
    editorial:{$exists: true}
    })
```

1. Mostrar todos los docuemntos que no contengan el capo cantidad
``` json
db.libros.find({
    cantidad:{$exists: false}
    })
```


# Operador Type(permite preguntar si un determinado campo corresponde con un tipo)

[Content-Type](https://www.mongodb.com/docs/manual/reference/operator/query/type/#mongodb-query-op.-type)

1. Mostrar todos los documentos donde el precio sean dobles
double
``` json
db.libros.find({
    precio:{$type: 1},
})
```

int
``` json
db.libros.find({
    precio:{$type: 16},
})
```

``` json
db.libros.insertOne({
    _id: 11,
    titulo: 'IA',
    editorial: 'Terra',
    precio: 125.5,
    cantidad: 20
})
```

``` json
db.libros.find(
{precio:{$type:1}},{_id:0}
)

db.libros.find(
{precio:{$type:1}},{_id:0, cantidad: 0}
)
```

Insert Many (insertMany)

``` json
db.libros.insertMany([
 {
    _id: 12,
    titulo: 'IA',
    editorial: 'Terra',
    precio: 125, 
	cantidad: 20
  },
  {
    _id: 13,
    titulo: 'Python para todos',
    editorial: 2001,
    precio: 200, 
	cantidad: 30
  }]
  )
```

1. Seleccionar los documentos donde la editorial sea de tipo entero
``` json
db.libros.find({
    editorial: {$type:16}
}
)
```

2. Seleccionar los documentos donde la editorial sea string
``` json
db.libros.find({
    editorial: {$type:18}
    }
    )
    ```

## Practica de Consultas
1. Instalar las tools de mongodb
[]()

2. Cargar el json empleados(Debemos estar ubicados en la carpeta donde se encuentra el JSON empleados)

- En local:
    comando: 
        


show dbs


