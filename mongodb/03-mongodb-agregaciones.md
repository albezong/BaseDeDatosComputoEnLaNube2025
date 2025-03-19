# Agregaciones en MongoDB (Framework)

## Métodos para realizar agregaciones simples
- distinct(): Devueleve valores no duplicados
- countDocuments(): Cuenta los documentos en una coleccion
- estimatedDcocumentCount(): Cuenta de manera aproximada durante un periodo de tiempo

## Una agregation pipeline consta de una o mas etapas

1. Cada etapa realiza
Por ejemplo, una fase 
2. Los documentos que se generan en una fase pasan a la siguiente fase.
3. Puede devolver resultaos para grupos de documentos como totales, máximo, minimo, etc.

### Se utiliza la clausula "aggregate"
- Existe una serie de operadores que se pueden utilizar para realizar operaciones. Se tienen distintos tipos: etapa de 

## Metodos Simples: countDocuments() y distinct()
1. Contar los documentos de la coleccion libros
``` json
db.libros.countDocuments()
```

2. Contar los documentos de la editorial
``` json
db.libros.countDocuments({
    editorial:{$eq: "Terra"}
})

db.libros.countDocuments({
    editorial:"Terra"
})
```

3. Seleccionar mostrar todos los libros mostrando solamente la editorial
``` json
db.libros.find(
    {},
    {_id:0, editorial:1}
)
```
4. Mostrar todas las distintas editoriales
``` json
db.libros.distinct('editorial')
```

[Documentacion de Agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## $match. Una pipeline básica
## tienen funciones de etapa
``` json
db.libros.aggregate(
    [
        {
            $match:{editorial:"Terra"}
        }
    ]
)
```

## $project. Incluir y renombrar campos
``` json
db.libros.aggregate(
   [
    {
        $match:{editorial:'Terra'}
    },
    {
        $project:{
            _id:0,
            titulo:1, 
            precio:1, 
            NombreEditorial:"$editorial",
            editorial:1
        }
    }
   ]  
)
```

- Generado por MongoCompass
``` json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        precio: 1,
        cantidad: 1,
        NombreEditorial: "$editorial",
        editorial: 1
      }
  }
]
```

# Crear un campo calculado con #project
``` json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  }
]
```

## sort. Ordenaciones
``` json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        precio: 1
      }
  }
]
``` 

## $group.Agrupaciones

[Agrupaciones](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/)

-- Cuantos documentos hay por cada una de las editoriales
``` json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero Documentos": {
          $count: {}
        }
      }
  }
]
``` 

-- Cuantos documentos hay por cada una de las editoriales por 
-- numero de documentos de manera de descendentes





-- Utilizando Mongo Atlas Base de Datos sample_airbnb
-- Agrupar por tipo de propiedad, mostrando el numero de propiedades
-- y el promedio de sus precios

``` json
{
  _id: "$property_type",
  Numero: {
    $count: {}
  },
  Media:
  {
    $avg:"$price"
  }
}
``` 
- Operadores $set 
``` json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  }
]

``` 
-  Operador $unset

``` json
[
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      /**
       * field: The field name
       * expression: The expression.
       */
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      /**
       * Provide the field name to exclude.
       * To exclude multiple fields, pass the field names in an array.
       */
      ["Media", "Media_Total"]
  }
]

-- Quitando solo un campo

[
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      /**
       * field: The field name
       * expression: The expression.
       */
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      /**
       * Provide the field name to exclude.
       * To exclude multiple fields, pass the field names in an array.
       */
      "Media"
  }
]
```
- Creando nuevas colecciones utilizando el operador $out
-- Nota: debe ser el último en la agregación

[
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      /**
       * field: The field name
       * expression: The expression.
       */
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      /**
       * Provide the field name to exclude.
       * To exclude multiple fields, pass the field names in an array.
       */
      "Media"
  },
  {
    $out:
      /**
       * Provide the name of the output database and collection.
       */
      {
        db: "sample_airbnb",
        coll: "media_propiedades"
        /*
    timeseries: {
    timeField: 'field',
    bucketMaxSpanSeconds: 'number',
    granularity: 'granularity'
    }
    */
      }
  }
]
```
- Ejemplos con operadores de comparación y lógicos

```json
[
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $gt: ["$price", 300]
        },
        medio: {
          $and: [
            {
              $gte: ["$price", 100]
            },
            {
              $lte: ["$price", 300]
            }
          ]
        },
        baratito: {
          $lt: ["$price", 100]
        }
      }
  }
]

```

- $cond - Devuelve valores según una condición (es parecido a un operador ternario de un lenguaje de programación)

``` Json
[
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $cond: [
            {
              $gt: ["$price", 300]
            },
            "Si",
            "No"
          ]
        },
        medio: {
          $cond: [
            {
              $and: [
                {
                  $gte: ["$price", 100]
                },
                {
                  $lte: ["$price", 300]
                }
              ]
            },
            "Si",
            "No"
          ]
        },
        baratito: {
          $cond: [
            {
              $lt: ["$price", 100]
            },
            "Si",
            "No"
          ]
        }
      }
  }
]


- Views


db.createView("ganancias_libros", 
"libros",
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Biblio"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre de Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        "Total de Ganancias": -1
      }
  }
])

db["ganancias_libros"].find(
  {"Total de Ganancias":{$lte:240}}
  ,{titulo:1, 'Total de Ganancias':1}).sort({titulo:-1})

  ```
<!-- 
## $match. Una pipeline básica
## Tiene funciones de etapa
``` json
db.libros.aggregate([
    {$match:{
        editorial: "Terra"
    }}
])
```

## $project. Incluir i renombrar campos 
``` json
db.libros.aggregate([
    {
        $match:{editorial: "Terra"}
    },
    {
        $project:{
            _id:0,
            titulo:1,
            precio:1,
            NombreEditorial: "$editorial",
            editorial:1
        }
    }
])
```

# $groupBy
[Agregaciones]()

-- Cuantos documentos hay por cada una de las editoriales
``` json

db.libros.aggregate(  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$editorial",
        "Numero Documento": {
          $count: {},
        },
      },
  },)

```

-- Cuantos documentos hay por cada una de las editoriales por numero de documentos de manera descendente
``` json
```

----*** UTILIZANDO MONGO ATLAS BASE DE DATOS AIRBNB

-- 

``` json
```

``` json
db.libros.({
    
})
```





-- 
``` json
/**
 * specifications: The fields to
 *   include or exclude.

Esto va en una tabla o bd airbnb
 
 */
{
  _id: 0,
  price: 1,
  name: 1,
  room_type: 1,
  caro: {
    $gt: ["price", 300],
  },
  medio: {
    $and: [
      { $gt: ["$price", 100] },
      { $lte: ["$price", 300] },
    ],
  },
  baratito,
}
```

-- $cond - Devuelve valores segu una condicion (es parecido a un operador ternario de un lenguaje de programacion)
``` json
/**
* specifications: The fields to
*   include or exclude.
Esto va en una tabla o bd airbnb
*/
{
  _id: 0,
  price: 1,
  name: 1,
  room_type: 1,
  caro: {
    $cond:[
      {
        $gt:["$price", 300]
      },'Si','No'
    ]
  },
  
  medio: {
    //Esto es un ternario
    $cond:[{
      $and: [
        {$gt: ["$price", 100]},
        {$lte: ["$price", 300]},
      ]
    },'Si','No']
  },
  baratito: {
    $cond:[{
    	$lt: ["$price", 100],
    },'Si','No']
  },
}
```

-- 
``` json
db.createView("ganancias_libros", "libros",
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Biblio"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre de editorial": "$editorial",
        "Total de ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        "Total de Ganancias": -1
      }
  }
]
```
-->


