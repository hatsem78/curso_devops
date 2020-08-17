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

    
Para construir una imagen debems crear el DockerFile dentro de nuestro proyecto


    FROM python:alpine3.10 # imagen oficial de python
    WORKDIR /app # se crea una carpeta contenedora dentro de la imagen
    COPY . /app # copia los contenido dentro de la carpeta app
    RUN pip install -r requirements.txt # instala los requerimientos
    EXPOSE 5000 # expone un puerto al cual sirve el api
    CMD python ./launch.py # comando que lanza el api
    
    #COPY requirements.txt /app/requirements.txt
    #ENTRYPOINT ["python", "./launch.py"]

    # Para construir la imagen se utiliza el siguiernte comando 
    docker build -t nombre-repositorio:version
    
    # recordar que cuando utilizamos el comando build simepre va un punto al final de las sentencia
    docker build -t in28min/hello-world-python:0.0.2.RELEASE .
    
    # Para ver la imagen 
    docker images 
    
    REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
    in28min/hello-world-python   0.0.2.RELEASE       8627cf03123d        8 hours ago         119MB
    
    # en el caso de hacer una modificación en el codigo fuente se debe volver a construir la imagen
     docker build -t in28min/hello-world-python:0.0.2.RELEASE .
     
     docker images
    
    CONTAINER ID        IMAGE                                      COMMAND                  CREATED             STATUS              PORTS                    NAMES
    c0cf51f3e17f        in28min/hello-world-python:0.0.2.RELEASE   "/bin/sh -c 'python …"   11 seconds ago      Up 9 seconds        0.0.0.0:5000->5000/tcp   adoring_poincare
    
Una manera de idenficar los containers aparte por su id es por el nombre el cual lo podemos proporcionar al arranque
    
    # Ejemplo poner un nombre a un container
     docker run -p 8001:8000 --name=currency-exchang in28min/currency-exchange:0.0.1-RELEASE
     
    docker container ls -a
    CONTAINER ID        IMAGE                                      COMMAND                  CREATED             STATUS                        PORTS               NAMES
    c75f5b140a06        in28min/currency-exchange:0.0.1-RELEASE    "sh -c 'java $JAVA_O…"   2 minutes ago       Exited (130) 18 seconds ago                       currency-exchang

    # Podemos también eliminar el contenedor con el nombre
    docker container rm currency-exchang
    
    
 
# Docker Networking(redes)

En docker tenemos algo fundamental que son las redes con lo cual podemos listar tres tipos de redes.
    
    # Listar todas las redes
    docker network ls
    NETWORK ID          NAME                                    DRIVER              SCOPE
    f085f8941180        bridge                                  bridge              local
    64b72a5ef90e        host                                    host                local
    94fa0bc3a432        none                                    null                local
    
la red bridge es para trabajar en entornos de desarrollos y no en producción se pueden crear network(redes) propias
    
    # crear una red
    docker create currency-network
    
    # listamos las redes
    docker network ls
    NETWORK ID          NAME                                    DRIVER              SCOPE
    f085f8941180        bridge                                  bridge              local
    3cd4529f3ad1        currency-network                        bridge              local
    64b72a5ef90e        host                                    host                local
    94fa0bc3a432        none                                    null                local

    
        
    
más sobre redes y practica https://docs.docker.com/network/network-tutorial-standalone/

# Docker-compose

Con Compose puedes crear diferentes contenedores y al mismo tiempo, en cada contenedor, diferentes servicios, unirlos a un volúmen común, 
iniciarlos y apagarlos, etc. Es un componente fundamental para poder construir aplicaciones y microservicios.
   El siguiente docker-compose.yml está compuesto por dos contenedores interconectado por una red customisada.
   
    version: '3.7' # versión de python utilizada
        services:
          currency-exchange: # Nombre del servicio 
            image: in28min/currency-exchange:0.0.1-RELEASE
            ports:
              - "8000:8000" # Puerto por el cual cual escucha y sirve la aplicación
            restart: always # especifica que siempre debe apagar y volvera levantar el servicio
            networks:
              - currency-compose-network # red a la cual esta conectado el contenedor
        
          currency-conversion:
            image: in28min/currency-conversion:0.0.1-RELEASE
            ports:
              - "8100:8100"
            restart: always
            environment:
              CURRENCY_EXCHANGE_SERVICE_HOST: http://currency-exchange
            depends_on:
              - currency-exchange
            networks:
              - currency-compose-network
          
        # Redes a ser creadas para facilitar la comunicación entre contenedores
        networks:
          currency-compose-network:
          
 intrucciones principales para el manejo de docker-compose
 
 <ul>
  <li>
    <p><em>docker-compose up</em>: da instrucciones a Docker para crear el contendor y ejecutarlo.</p>
  </li>
  <li>
    <p><em>docker-compose down</em>: apaga todo los servicios que levantó con docker-compose up.</p>
  </li>
  <li>
    <p><em>docker-compose ps</em>: permite ver los contenedores funcionando.</p>
  </li>
  <li>
    <p><em>docker-compose exec</em>: permite ejecutar un comando a uno de los servicios levantados de Docker-compose.</p>
  </li>
</ul>

    # Eliminamos todas las imagenes conetenedores y network
    docker system prune -a
    
    # Lanzamos los servicios 
    docker-compose up -d
    
    # Para ver la configuración del docker-compose el cual esta lanzado
    docker-compose config
    
    # Ver las images creadas con docker-compose
    docker-compose images
    
                 Container                        Repository                 Tag          Image Id      Size 
    ---------------------------------------------------------------------------------------------------------
    microservices_currency-conversion_1   in28min/currency-conversion   0.0.1-RELEASE   064d42f74ca3   151 MB
    microservices_currency-exchange_1     in28min/currency-exchange     0.0.1-RELEASE   1ec056fbf00c   146 MB

    
    # docker-compose ps permite ver los contenedores funcionando.
    docker-compose ps

                   Name                              Command               State           Ports         
    -----------------------------------------------------------------------------------------------------
    microservices_currency-conversion_1   sh -c java $JAVA_OPTS -Dja ...   Up      0.0.0.0:8100->8100/tcp
    microservices_currency-exchange_1     sh -c java $JAVA_OPTS -Dja ...   Up      0.0.0.0:8006->8000/tcp
    
    # una descripcion mas detallada de cada proceso
    docker-compose top
    
    microservices_currency-conversion_1
    UID     PID    PPID    C   STIME   TTY     TIME                                CMD                            
    --------------------------------------------------------------------------------------------------------------
    root   15344   15315   7   11:59   ?     00:01:15   java -Djava.security.egd=file:/dev/./urandom -jar /app.jar
    
    microservices_currency-exchange_1
    UID     PID    PPID    C   STIME   TTY     TIME                                CMD                            
    --------------------------------------------------------------------------------------------------------------
    root   15206   15174   7   11:59   ?     00:01:16   java -Djava.security.egd=file:/dev/./urandom -jar /app.jar


# Kubernetes

Implementación cloud google.

https://cloud.google.com/

kubectl es el comando para realizar las distintas intrucciones que realizamos en google

para crear un diplyment basado en una imagen docker 

    kuberclt create deplyment nombre-imagen --image=imagen-docker
    hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE
    
    cuando queremos expooner el rest-api debemos ejecutar el sigueinte comando
    kubectl expose deplyment nombre-imagen --type=LoadBalancer --port=8080
    
    Esto nos expone el rest-api a internet
    kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080
    
    Comandos utiles para saber como estan los distintos deployments creados
    
    # Para ver los eventeos realizados ultimamente
    kubectl get events 
    
    # Nos informa de los diplyments creados 
    kubectl get deplyments
    
    NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
    hello-world-rest-api   1/1     1            1           128m
    
    # Para ver el estado de los distintos deplyments 
    kubctl get pod
    
    NAME                                    READY   STATUS    RESTARTS   AGE
    hello-world-rest-api-6f58bf7f76-m5hsv   1/1     Running   0          129m
    
## Pods
Los Pods son las unidades de computación desplegables más pequeñas que se pueden crear y gestionar en Kubernetes.

¿Qué és un Pod?

Un Pod (como en una vaina de ballenas o vaina de guisantes) es un grupo de uno o más contenedores (como contenedores Docker), 
con almacenamiento/red compartidos, y unas especificaciones de cómo ejecutar los contenedores. Los contenidos de un Pod son siempre coubicados, 
coprogramados y ejecutados en un contexto compartido. Un Pod modela un "host lógico" específico de la aplicación: contiene uno o más contenedores de aplicaciones relativamente entrelazados. 
Antes de la llegada de los contenedores, ejecutarse en la misma máquina física o virtual significaba ser ejecutado en el mismo host lógico. Más especificaciones

https://kubernetes.io/es/docs/concepts/workloads/pods/pod/


## ReplicaSet
El propósito de un ReplicaSet es mantener un conjunto estable de Pods de réplica ejecutándose en un momento dado. Como tal, 
a menudo se usa para garantizar la disponibilidad de un número específico de Pods idénticos.

    kubectl get replicasets
    # A continuación, puede implementar los ReplicaSets actuales:
    kubectl get rs
    NAME                              DESIRED   CURRENT   READY   AGE
    hello-world-rest-api-6f58bf7f76   1         1         1       8d
    
    kubectl get pods -o wide
    
    NAME                                    READY   STATUS    RESTARTS   AGE   IP          NODE                                                 NOMINATED NODE   READINESS GATES
    hello-world-rest-api-6f58bf7f76-m5hsv   1/1     Running   0          8d    10.36.1.6   gke-in28minutes-claster-default-pool-8086b58c-jvqc   <none>           <none>
    
    # podemos eliminar el pod con la informacion anteriror
    kubectl delete pods hello-world-rest-api-6f58bf7f76-m5hsv
    
    # Para replicar 3 instancias 
    kubectl scale deployment hello-world-rest-api --replicas=3
    
    deployment.extensions/hello-world-rest-api scaled
    
    # verificamos los pods creados
    kubectl get pods
    
    NAME                                    READY   STATUS    RESTARTS   AGE
    hello-world-rest-api-6f58bf7f76-6dgl9   1/1     Running   0          89s
    hello-world-rest-api-6f58bf7f76-7cc8w   1/1     Running   0          5m35s
    hello-world-rest-api-6f58bf7f76-gq67r   1/1     Running   0          89s
    
    kubectl get replicaset
    NAME                              DESIRED   CURRENT   READY   AGE
    hello-world-rest-api-6f58bf7f76   3         3         3       8d
    
    # Podemos verificar los ulitmos eventos con get events
    kubectl get events
    
    LAST SEEN   TYPE     REASON              OBJECT                                       MESSAGE
    7m47s       Normal   Scheduled           pod/hello-world-rest-api-6f58bf7f76-6dgl9    Successfully assigned default/hello-world-rest-api-6f58bf7f76-6dgl9 to gke-in28minutes-
    claster-default-pool-8086b58c-ndlk
    7m45s       Normal   Pulling             pod/hello-world-rest-api-6f58bf7f76-6dgl9    Pulling image "in28min/hello-world-rest-api:0.0.1.RELEASE"
    7m42s       Normal   Pulled              pod/hello-world-rest-api-6f58bf7f76-6dgl9    Successfully pulled image "in28min/hello-world-rest-api:0.0.1.RELEASE"
    7m40s       Normal   Created             pod/hello-world-rest-api-6f58bf7f76-6dgl9    Created container hello-world-rest-api
    7m39s       Normal   Started             pod/hello-world-rest-api-6f58bf7f76-6dgl9    Started container hello-world-rest-api
    11m         Normal   Scheduled           pod/hello-world-rest-api-6f58bf7f76-7cc8w    Successfully assigned default/hello-world-rest-api-6f58bf7f76-7cc8w to gke-in28minutes-
    claster-default-pool-8086b58c-jvqc
    11m         Normal   Pulled              pod/hello-world-rest-api-6f58bf7f76-7cc8w    Container image "in28min/hello-world-rest-api:0.0.1.RELEASE" already present on machine
    11m         Normal   Created             pod/hello-world-rest-api-6f58bf7f76-7cc8w    Created container hello-world-rest-api
    11m         Normal   Started             pod/hello-world-rest-api-6f58bf7f76-7cc8w    Started container hello-world-rest-api
    7m47s       Normal   Scheduled           pod/hello-world-rest-api-6f58bf7f76-gq67r    Successfully assigned default/hello-world-rest-api-6f58bf7f76-gq67r to gke-in28minutes-
    claster-default-pool-8086b58c-m2zw
    7m45s       Normal   Pulling             pod/hello-world-rest-api-6f58bf7f76-gq67r    Pulling image "in28min/hello-world-rest-api:0.0.1.RELEASE"
    7m40s       Normal   Pulled              pod/hello-world-rest-api-6f58bf7f76-gq67r    Successfully pulled image "in28min/hello-world-rest-api:0.0.1.RELEASE"
    7m39s       Normal   Created             pod/hello-world-rest-api-6f58bf7f76-gq67r    Created container hello-world-rest-api
    7m38s       Normal   Started             pod/hello-world-rest-api-6f58bf7f76-gq67r    Started container hello-world-rest-api
    11m         Normal   Killing             pod/hello-world-rest-api-6f58bf7f76-m5hsv    Stopping container hello-world-rest-api
    11m         Normal   SuccessfulCreate    replicaset/hello-world-rest-api-6f58bf7f76   Created pod: hello-world-rest-api-6f58bf7f76-7cc8w
    7m47s       Normal   SuccessfulCreate    replicaset/hello-world-rest-api-6f58bf7f76   Created pod: hello-world-rest-api-6f58bf7f76-gq67r
    7m47s       Normal   SuccessfulCreate    replicaset/hello-world-rest-api-6f58bf7f76   Created pod: hello-world-rest-api-6f58bf7f76-6dgl9
    7m47s       Normal   ScalingReplicaSet   deployment/hello-world-rest-api              Scaled up replica set hello-world-rest-api-6f58bf7f76 to 3
    
    # podemos ordenar la salida por el tiempo
    kubectl get events --sort-by=.metadata.creationTimestamp
    
    LAST SEEN   TYPE     REASON              OBJECT                                       MESSAGE
    15m         Normal   Killing             pod/hello-world-rest-api-6f58bf7f76-m5hsv    Stopping container hello-world-rest-api
    15m         Normal   SuccessfulCreate    replicaset/hello-world-rest-api-6f58bf7f76   Created pod: hello-world-rest-api-6f58bf7f76-7cc8w
    15m         Normal   Scheduled           pod/hello-world-rest-api-6f58bf7f76-7cc8w    Successfully assigned default/hello-world-rest-api-6f58bf7f76-7cc8w to gke-in28minutes-
    claster-default-pool-8086b58c-jvqc
    15m         Normal   Pulled              pod/hello-world-rest-api-6f58bf7f76-7cc8w    Container image "in28min/hello-world-rest-api:0.0.1.RELEASE" already present on machine
    15m         Normal   Created             pod/hello-world-rest-api-6f58bf7f76-7cc8w    Created container hello-world-rest-api
    15m         Normal   Started             pod/hello-world-rest-api-6f58bf7f76-7cc8w    Started container hello-world-rest-api
    11m         Normal   Scheduled           pod/hello-world-rest-api-6f58bf7f76-gq67r    Successfully assigned default/hello-world-rest-api-6f58bf7f76-gq67r to gke-in28minutes-
    claster-default-pool-8086b58c-m2zw
    11m         Normal   SuccessfulCreate    replicaset/hello-world-rest-api-6f58bf7f76   Created pod: hello-world-rest-api-6f58bf7f76-6dgl9
    11m         Normal   SuccessfulCreate    replicaset/hello-world-rest-api-6f58bf7f76   Created pod: hello-world-rest-api-6f58bf7f76-gq67r
    11m         Normal   Scheduled           pod/hello-world-rest-api-6f58bf7f76-6dgl9    Successfully assigned default/hello-world-rest-api-6f58bf7f76-6dgl9 to gke-in28minutes-
    claster-default-pool-8086b58c-ndlk
    11m         Normal   ScalingReplicaSet   deployment/hello-world-rest-api              Scaled up replica set hello-world-rest-api-6f58bf7f76 to 3
    11m         Normal   Pulling             pod/hello-world-rest-api-6f58bf7f76-gq67r    Pulling image "in28min/hello-world-rest-api:0.0.1.RELEASE"
    11m         Normal   Pulling             pod/hello-world-rest-api-6f58bf7f76-6dgl9    Pulling image "in28min/hello-world-rest-api:0.0.1.RELEASE"
    11m         Normal   Pulled              pod/hello-world-rest-api-6f58bf7f76-6dgl9    Successfully pulled image "in28min/hello-world-rest-api:0.0.1.RELEASE"
    11m         Normal   Created             pod/hello-world-rest-api-6f58bf7f76-6dgl9    Created container hello-world-rest-api
    11m         Normal   Pulled              pod/hello-world-rest-api-6f58bf7f76-gq67r    Successfully pulled image "in28min/hello-world-rest-api:0.0.1.RELEASE"
    11m         Normal   Started             pod/hello-world-rest-api-6f58bf7f76-6dgl9    Started container hello-world-rest-api
    11m         Normal   Created             pod/hello-world-rest-api-6f58bf7f76-gq67r    Created container hello-world-rest-api
    11m         Normal   Started             pod/hello-world-rest-api-6f58bf7f76-gq67r    Started container hello-world-rest-api


# Understanding Deployment in Kubernetes

Vamos a comprender como es el deployment de las deistintas versiones de los programas. Más información:
https://v1-18.docs.kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment

    # listamos replicaset
    kubectl get rs
    NAME                              DESIRED   CURRENT   READY   AGE
    hello-world-rest-api-6f58bf7f76   3         3         3       8d
    
    # vemos los distintos replicaciones con la ip y mas
    kubectl get pods -o wide
    
    NAME                                    READY   STATUS    RESTARTS   AGE   IP          NODE                                                 NOMINATED NODE   READINESS GATES
    hello-world-rest-api-6f58bf7f76-6dgl9   1/1     Running   0          26m   10.36.2.7   gke-in28minutes-claster-default-pool-8086b58c-ndlk   <none>           <none>
    hello-world-rest-api-6f58bf7f76-7cc8w   1/1     Running   0          30m   10.36.1.7   gke-in28minutes-claster-default-pool-8086b58c-jvqc   <none>           <none>
    hello-world-rest-api-6f58bf7f76-gq67r   1/1     Running   0          26m   10.36.0.5   gke-in28minutes-claster-default-pool-8086b58c-m2zw   <none>           <none>
    
    kubectl get rs -o wide
    NAME                              DESIRED   CURRENT   READY   AGE   CONTAINERS             IMAGES                                       SELECTOR
    hello-world-rest-api-6f58bf7f76   3         3         3       8d    hello-world-rest-api   in28min/hello-world-rest-api:0.0.1.RELEASE   app=hello-world-rest-api,pod-template
    -hash=6f58bf7f76
    
    # nuevamente con el control vemos si hay algun error hello-world-rest-api-54bd4b9cfc-5zqjj   0/1     InvalidImageName
    kubectl get rs -o wide
    NAME                                    READY   STATUS             RESTARTS   AGE
    hello-world-rest-api-54bd4b9cfc-5zqjj   0/1     InvalidImageName   0          25h
    hello-world-rest-api-6f58bf7f76-6dgl9   1/1     Running            0          25h
    hello-world-rest-api-6f58bf7f76-7cc8w   1/1     Running            0          25h
    hello-world-rest-api-6f58bf7f76-gq67r   1/1     Running            0          25h
    
    # podemos ver una descripción completa y la descripción del error
    kukectl describe pod hello-world-rest-api-54bd4b9cfc-5zqjj
    
    Events:
      Type     Reason         Age                     From                                                         Message
      ----     ------         ----                    ----                                                         -------
      Warning  InspectFailed  24m (x6841 over 25h)    kubelet, gke-in28minutes-claster-default-pool-8086b58c-jvqc  Failed to apply default image tag "DUMMY_IMAGE:TEST": couldn't
     parse image reference "DUMMY_IMAGE:TEST": invalid reference format: repository name must be lowercase
      Warning  Failed         4m58s (x6934 over 25h)  kubelet, gke-in28minutes-claster-default-pool-8086b58c-jvqc  Error: InvalidImageName
      
    kubectl get events --sort-by=.metadata.creationTimestamp
    
    
    LAST SEEN   TYPE      REASON          OBJECT                                      MESSAGE
    29m         Warning   InspectFailed   pod/hello-world-rest-api-54bd4b9cfc-5zqjj   Failed to apply default image tag "DUMMY_IMAGE:TEST": couldn't parse image reference "DUMMY
    _IMAGE:TEST": invalid reference format: repository name must be lowercase
    4m55s       Warning   Failed          pod/hello-world-rest-api-54bd4b9cfc-5zqjj   Error: InvalidImageName
    
    
## Understanding Services in Kubernetes

Una forma abstracta de exponer una aplicación que se ejecuta en un conjunto de Pods como un servicio de red.

Con Kubernetes, no necesita modificar su aplicación para usar un mecanismo de descubrimiento de servicios desconocido. 
Kubernetes les da a los pods sus propias direcciones IP y un solo nombre de DNS para un conjunto de pods, 
y puede equilibrar la carga entre ellos




## Understanding Kubernetes Architecture - Master Node and Nodes


 El nodo maestro administra el clúster de Kubernetes y es el punto de entrada para todas las tareas administrativas. 
Puede hablar con el nodo principal a través de CLI, GUI o API. Para lograr la tolerancia a fallas, puede haber más de un nodo maestro en el clúster. Cuando tenemos más de un nodo maestro, habría un modo de alta disponibilidad y un líder realizaría todas las operaciones. 
Todos los demás nodos maestros serían seguidores de ese distributed key-value store.

### API Server

API Server realiza todas las tareas administrativas en el nodo principal. Un usuario envía los comandos restantes al servidor API, que luego valida las solicitudes, luego las procesa y ejecuta. etcd guarda el estado resultante del clúster como un almacén de clave-valor distribuido.


### Scheduler

359/5000
Después de eso, tenemos un programador. Entonces, como sugiere el nombre, el programador programa el trabajo en diferentes nodos trabajadores. Tiene la información de uso de recursos para cada nodo trabajador. El planificador también considera los requisitos de calidad del servicio, la ubicación de los datos y muchos otros parámetros similares. Luego, el programador programa el trabajo en términos de pods y servicios.


### Controller Manager

El Control Manager gestiona los bucles de control no terminantes que regulan el estado del clúster de Kubernetes. Ahora, cada uno de estos lazos de control conoce el estado deseado del objeto que administra y luego observa su estado actual a través de los servidores API.

### etcd

Etcd es un almacén de clave-valor distribuido que se utiliza para almacenar el estado del clúster. Por lo tanto, debe ser parte del maestro de Kubernetes o también puede configurarlo externamente. etcd está escrito en goLang y se basa en el algoritmo de consenso de Raft.

La balsa permite que la colección de máquinas funcione como un grupo coherente que puede sobrevivir a las fallas de algunos de sus miembros. Incluso si algunos de los miembros no funcionan, este algoritmo puede funcionar en cualquier momento. Uno de los nodos del grupo será el maestro y el resto serán los seguidores.

Solo puede haber un maestro, y todos los demás maestros deben seguir a ese maestro. Además de almacenar el estado del clúster, etcd también se utiliza para almacenar los detalles de configuración, como las subredes y los mapas de configuración.

### Worker Node

Un nodo trabajador es un servidor virtual o físico que ejecuta las aplicaciones y está controlado por el nodo maestro. Los pods se programan en los nodos trabajadores, que tienen las herramientas necesarias para ejecutarlos y conectarlos. Las vainas no son más que una colección de contenedores.

Y para acceder a las aplicaciones desde el mundo externo, debe conectarse a los nodos trabajadores y no a los nodos maestros.

### Container Runtime

El tiempo de ejecución del contenedor se usa básicamente para ejecutar y administrar un ciclo de vida continuo en el nodo trabajador. Algunos ejemplos de tiempos de ejecución de contenedores que puedo darle son los contenedores rkt, lxc, etc. A menudo se observa que Docker también se conoce como tiempo de ejecución de contenedores, pero para ser precisos, déjeme decirle que Docker es una plataforma que usa contenedores. como tiempo de ejecución del contenedor.

### Kubelet

Kubelet es básicamente un agente que se ejecuta en cada nodo trabajador y se comunica con el nodo maestro. Entonces, si tiene diez nodos trabajadores, kubelet se ejecuta en cada nodo trabajador. Recibe la definición de pod por varios medios y ejecuta los contenedores asociados con ese puerto. También se asegura de que los envases que forman parte de las vainas estén siempre sanos.

El kubelet se conecta al tiempo de ejecución del contenedor usando el marco gRPC. El kubelet se conecta a la interfaz de tiempo de ejecución del contenedor (CRI) para realizar contenedores y operaciones de imagen. El servicio de imágenes es responsable de todas las operaciones relacionadas con las imágenes, mientras que el servicio de tiempo de ejecución es responsable de todas las operaciones relacionadas con el contenedor y el pod. Estos dos servicios tienen dos operaciones diferentes que realizar.

### Kube-proxy

Kube-proxy se ejecuta en cada nodo trabajador como proxy de red. Escucha el servidor API para cada creación o eliminación de puntos de servicio. Para cada punto de servicio, kube-proxy establece las rutas para que pueda llegar a él.






    
    