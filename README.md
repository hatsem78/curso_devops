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
    
    
    
 
    





