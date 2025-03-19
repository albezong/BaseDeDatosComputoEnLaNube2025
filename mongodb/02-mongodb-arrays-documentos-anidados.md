# Busquedas en Arrays y Documentos Anidados

## Buscar dentro de un documento anidado

``` json
db.ciudades.find({
    "alcalde.nombre": "Crystal"
})
```

1. Seleccionar las ciudades donde los alcaldez tengan mas de 70 años
``` json
db.ciudades.find({
    "alcalde.edad": { $gt: 70 }
})
```

2. Realiza la consulta anterior pero realizando una proyeccion
``` json
db.ciudades.find({
    "alcalde.edad": { $gt: 70 }},
    {_id: 0, nombre:1,"alcalde.edad":1
})
```

3. Seleccionar la ciudades donde el alcanlde tenga 76 años o 41
``` json
db.ciudades.find({
    $or: [{ "alcalde.edad": { $gt: 70 } },
    { "alcalde.edad": { $gt: 41 } }
    ]
})
```

## Consultas con Arrays

1. Crear una coleccion cazas db.
2. Cargar los documentos de la coleccion cazas, de la data cazas

## Ejemplos con consulta con Arrays
1. Busca los documentos que tengan usa como paises
``` json
db.cazas.find({ paises: "usa"})
```

2. Buscar los documentos donde cualquier elemento del array dimensiones sea mayor a 5
``` json
db.cazas.find({
    dimensiones:($gt:5)
})
```

3. Buscar los documentos donde las dimensiones del avion por ancho que es la posicion 1, sea superior a 14
``` json
db.cazas.find({
    "dimensiones.1":{$eq:14}
    })
```

``` json
db.cazas.find({
    "dimensiones.1":{$eq:14}
    },{ _id:0,modelo:1,dimensiones:1})
```

### Arrays. Operador $all

1. Buscar los aviones que estan sirviendo en Egipto y en Israel.
``` json
db.cazas.find({ 
    paises: {$all:['egipto', 'israel']}
    })
```
***con proyeccion***
``` json
db.cazas.find({ 
    paises: {$all:['egipto', 'israel']}
    },
    {
        _id: 0, 
        modelo: 1,
        paises: 1
    }
    )
```

2. Seleccionar los cazas que sirven en Inglaterra y España
``` json
db.cazas.find({
    $and: [
        {paises: 'inglaterra'},
        {paises: 'españa'}
    ]
})
```
***con all***
``` json
db.cazas.find({
    paises: {$all: ['inglaterra', 'e.spaña']}
})
```

### Arrays Operador $elemMatch -> Permite hacer Busquedas dentro de arrays, este busca la lista de condiciones que esta  dentro del perador (Esto solo para arreglos)

1. Buscar que aviones tienen uso dentro de inglaterra
``` json
db.cazas.find({
    paises:{
        $elemMatch:{$eq:'Inglaterra'}
    }
})
```

2. Bucar los aviones donde las dimensiones sean menores a 20 o mayores a 15
``` json
db.cazas.find({
    dimensiones:{
        $elemMatch:{$lt:20, $gt:15}
    }
})
```

### Buscar en arrays de documentos Anidados
1. Utilizar la coleccion ciudades
2. Buscar los consejeros de nombre Jeri Flower de edad 78
``` json
db.ciudades.find({
    consejeros:{
        identificador: 1, nombre: "Jeri Flowers", edad: 78
    }
})
```

3. Buscar en consenjeros donde tengan una edad mayor a 70
``` json
db.ciudades.find({"consejeros.edad":{$gt:70}})
```

4. Buscar en consenjeros en la posicion 2 que esten entre 70 y 80 años
``` json
db.ciudades.find({
    "consenjeros.edad":{
        $gt:70, $lt:80
    }
},
{
    _id:0, "consejeros.nombre":1, "consejeros.edad":1
}
)
```

5. Buscar los consejeros de la posicion 0 que sean mayores a 70
``` json
db.ciudades.find({
    "consenjeros.0.edad":{
        $gt:70
    }
},
{
    _id:0, nombre:1, edad:1
}
)
```

6. Buscar los consejeros que sean mayores a 70
``` json
db.ciudades.find({
    consenjeros:{
        $gt:70
    }
})
```

/*
en donde esta .git abrir git bash

git status
git status
git commit
"Logro: Consultas con documentos anidados, arrays y documentos dentro de Arrays Terminados"
git log 
git push

*/


