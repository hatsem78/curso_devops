# Resumen curso devps

## Docker

ejemplo para arrancar un imagen docker

docer run -p 5000:5000 in28min/hello-worLd-python:0.0.1.RELEASE

-p 5000:5000 corresponde al puerto 

hello-worLd-python es la image que queremos arrancar 
:0.0.1.RELEASE es la version tag que queremos correr es como el git que se crea los tag
los comandos utitlizados en el primer capitulos estan en command_docker

para poder correr varias imagenes in una sola terminal se agrega -d
    docker run -d -p 5000:5000 IMAGES
genera un logs que luego podemos ir viendo que sucede con el log de la aplicación con el comando
docker logs c66921f6594bce205420efe168ceab8dd127a1c806ada555d3dae24d7a3b55dc
también podemos ver el logs con solo los tres primeros caracteres
    docker log c666
Si queremos ver todos los containers que estan corriendo sería.
    docker container ls
Para ver todos los container cons us correspodniente estado.
    docker container ls -a
Para parar un container se puede hacer de dos formas una con el id completo o con los 4 primeros caracteres
    # con el ide completo
    docker stop a6db7478ed0a
    # con los 4 primeros caracteres
    docker stop a6db
cuando utilizamos el comando pull lo que hacemos nos vajamos una imagen oficial ejemplo msql

    # baja la imagen de mysql pero no la activa también podes especificar la ultima versión
    docker pull mysql
    
    # ultima versión
    docker pull mysql:latest
    # Para espcificar una versión en particular
    docker pull mysql:0.0.1
    
    # Para inspecionar el contenido de una imagen
     docker inspect mysql
    
    # Elimnar una imagen en el caso de mysql primero obtenemos el id container
    docker containers ls -a
    REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
    mysql                             latest              9228ee8bac7a        10 days ago         547MB
    
    docker image remove 9228ee8bac7a
    
    # Para remover un container lo primero tenemos que ver si esta corriendo o no para esto
    docker container ls
    CONTAINER ID        IMAGE                                        COMMAND                  CREATED             STATUS              PORTS                              NAMES
    8b0e96b59c3a        in28min/hello-world-rest-api:0.0.1.RELEASE   "sh -c 'java -jar /a…"   15 hours ago        Up 15 hours         8000/tcp, 0.0.0.0:5001->8080/tcp   great_poitras
    
    # Paramo el servicio id completo 
    docker container stop 8b0e96b59c3a
    # o con solo los cuatros caracteres
    docker container stop 8b0e
    # Ahora podemos remover dicho container
    docker container rm 8b0e96b59c3a
    # Tambien con el id abreviado
    docker container rm 8b0e
Algunas veses queremos arrancar de cero con los containers, para esto podemos depurar todos los continer con el comando prune
    # Elimina todos los containers
    docker container prune
    
    
Algunos comandos interesante con respecto al docker que nos brinda información con respecto al consumo de cpu, tamaño etc

    # el comando system df nos informa 
    TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
    Images              5                   1                   576MB               459.9MB (79%)
    Containers          1                   1                   32.77kB             0B (0%)
    Local Volumes       16                  0                   38.32MB             38.32MB (100%)
    Build Cache         0                   0                   0B                  0B
    
    # otro comando interesante dentro de system es stats que nos brinda la información de consumo se cp como ejemplo
    # listamos lso containers que estan lanzados
    docker container ls
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
    1a27a05b5eab        100229ba687e        "sh -c 'java -jar /h…"   5 minutes ago       Up 5 minutes        0.0.0.0:5000->5000/tcp   hardcore_noether
    
    # con el comando stats nos brinda ls siguiente infomrmación ejemplo consumo cpu.
    # recordar que podemos ingresar los cuatros primoros caracteres del id container para un container en especifico
    docker stats 1a27
    
    CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT   MEM %               NET I/O             BLOCK I/O           PIDS
    1a27a05b5eab        hardcore_noether    0.25%               252MiB / 8.332GiB   2.95%               14.8kB / 1.93kB     991kB / 0B          28
    
    # ingresando solamanet statas nos brinda la infomración de todos los contenedores lanzados
    docker stats 
    
    CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
    dd0be7d603bf        modest_turing       0.00%               16.24MiB / 8.332GiB   0.19%               4.31kB / 0B         27.9MB / 0B         6
    1a27a05b5eab        hardcore_noether    0.19%               252MiB / 8.332GiB     2.95%               14.8kB / 1.93kB     991kB / 0B          28

    


    
 
    





