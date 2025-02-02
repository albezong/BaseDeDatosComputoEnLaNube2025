# MARIADB
#

## CONTENEDOR DE MARIADB
``` json
docker container run`
--name maria-db`
-d -p 3344:3306`       //>>>-d -p 3304:puertodemariadb´****sin volumen
--env MARIADB_USER=example-user `
--env MARIADB_PASSWORD=user-password `
--env MARIADB_ROOT_PASSWORD=root-secret-password `
--env MARIADB_DATABASE=world-db `
mariadb-jammy
```

## COMO HACER UN VOLUMEN
docker volume ls /*para ver cuantos volumenes tienes
docker volume create volumen-mariadb

/*para eliminar contenedor*/
docker ps
docker container stop (nombre o ID)
docker ps
docker ps -a
docker container rm (nombre o ID)

## VOLUMEN NOMBRADO
docker container run ´
--name maria-db´
-d -p 3344:3306´///////-d -p 3304:puertodemariadb´****sin volumen
--env MARIADB_USER=example-user `
--env MARIADB_PASSWORD=user-password `
--env MARIADB_ROOT_PASSWORD=root-secret-password `
--env MARIADB_DATABASE=world-db `
--volume volumen-mariadb:/var/lib/mysql´
mariadb:jammy


(docker engine en windows)



/*phpmyadmin*/
docker container run´
--name phpmyadmin1´
-dp 8090:80´
-e PMA_´
phpmyadmin:5.0.2-apache

/*conectar phpmyadmin con base de datos*/
/*hacer una red
docker network --help
docker network create network1
docker network ls


--
/*postgres con pgadmin*/
/*instalar mysql*/
--


docker network --help
docker ps
docker network connect (phpmyadmin network1) contenedor
docker container inspect (nombre o ID del contenedor)
/*conectar con phpmyadmin*/


docker ps 
docker 
/*subir un servidor html*/