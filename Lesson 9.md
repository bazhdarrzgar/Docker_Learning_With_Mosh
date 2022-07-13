* installing docker conmpose

    - link 1: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-22-04

    - link 2: https://stackoverflow.com/questions/49839028/how-to-upgrade-docker-compose-to-latest-version

```bash

# installing in the github page: https://github.com/docker/compose/

mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.3.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose

curl -SL https://github.com/docker/compose/releases/download/v1.28.5/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose


chmod +x ~/.docker/cli-plugins/docker-compose

docker compose version

===================
Output
Docker Compose version v2.3.3
===================


```


------------------------------

* removing contianer and image in other technique

```bash

docker image ls -q

===================
58f2f789bbb6
73d68b2593af
90ed31added3
50bfd284aa0d
===================

docker container ls -aq

===================
fb2ef15eb07b
cdfee19c67e1
87ac47aced91
e7c524964bcf
===================



# process remove
# NOTE: if you want remove image you should first remove the container for the image after that remove the image
docker container rm -f $(docker container ls -aq)

===================
fb2ef15eb07b
cdfee19c67e1
87ac47aced91
e7c524964bcf
===================


docker image rm -f $(docker image ls -aq)

===================
Untagged: final-trans:latest
Deleted: sha256:58f2f789bbb633a27f7a1265f618a1fe33eeff6579edd8b5b22251a077c4f213
Deleted: sha256:fadf32cbfe09acee722fb97721fab28313989274ca12b57a5d8fd349a18d99a6
Deleted: sha256:a86037d0acb10feb43360138fde5aa878de420e0fb21411b87074079a0f03b87
.
.
.
===================


docker images

===================
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
===================

docker ps -a

===================
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
===================

docker ps

===================
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
===================


```

----------------------------

* bulid the project using docker compose

```bash

# docker compose version
docker compose version

===================
# it should be higher than 1.28.5
Docker Compose version v2.3.3
===================

# info about docker compose
docker compose

# build the project
docker compose up

# NOTE: you should know that in previous version docker compose we use ( docker-compose ) for using docker compose and for version docker compose we use ( docker-compose --version ) but in newer version docker compose this is completed change

```

### Note:

* you should be careful about dockerfile in the backend and front end because both of this file should be check about any problem if it is have when you are running if in first time the docker compose is failed then try again and try fix this problem in docker file but first start in docker compose file

----------------------------

* yml vs json

1. json

```json
# json have double quotes ("") for knonwing if something is string or not
# json have comman (,) for sprate the line
# json have big bracket ( {} ) for organize the component 
# json have boxy bracket ( [] ) for organizing the list
{
    "name": "The Ultimate Docker Course",
    "price": 149
    "is_published": true,
    "tags": ["software","devops"]
    "author": {
        "first_name": "Mosh",
        "last_name": "Hamedani"
    }
}

```

2. yml code

```yml
# first yml start with --- is beganning of the line
# yml not have double quotes ("")
# yml not have comman (,)
# yml not have big bracket ( {} )
# yml not have boxy bracket ( [] )
---
name: The Ultimate Docker Course
price: 149
is_published: true
tags:
    - software
    - devops
author:
    first_name: Mosh
    last_name: Hamedani

```

### Note:

* json is faster than yml because in json we know by double quotes what is string or knowing by using comma what is other line or what is list by using boxy bracket ... then parsing yml is faster than yml

* yml file is use for configuration file
* json use for exchange data in the internet between client and server

---------------------

* let's code with docker compose

    - default name docker compose is ( docker-compose.yml ) you should use this name if you want build the project with docker compose

```yml

# this is the website to know about docker compose file format link: https://docs.docker.com/compose/compose-file/compose-file-v3/
# you use this version for use the latest version of this tool that use in this file for the project
version: "3.8"
# we deal with server this is why i used this name for manage componenet server

services:
    # i used this name for manage componenet frontend
    web:
        # this is will build the image dockerfile that is inside frontend folder
        build: ./frontend
        ports:
            # port 3000 in my machine will lesson to port 3000 in the container that build with frontend
            - 3000:3000
    # i used this name for manage componenet backend
    api:
        # this is will build the image dockerfile that is inside backend folder
        build: ./backend
        # managing port 
        ports:
        # port 3001 in my machine will lesson to port 3001 in the container that build with backend
            - 3001:3001
        # we need environemt for knowing the database
        environment:
            # db is my database variable that have some component dabase is inside
            # vidly is my database name
            DB_URL: mongodb://db/vidly
            # - DB_URL=mongodb://db/vidly
    # i used this name for manage componenet database
    db:
        # i build the mongo db in top of xenial linux os
        image: mongo:4.0-xenial
        ports:
            # by default mongo db is lesson to port 27017 this is why i used the same port for my host and container
            - 27017:27017
        volumes:
            # by default mongo db is store the data of the database inside this directory ( /data/db )
            - vidly:/data/db

# we go back to service level to knowed that i use volumes that is vidly this is my name of folder it can be any name but make sure you change the volume for volumes inside database means the name
volumes:
    vidly:
    # - vidly:

```

* format the text for more understanding

```yml

version: "3.8"

services:
  frontend:
    depends_on: 
      - backend
    build: ./frontend
    ports:
      - 3000:3000

  backend: 
    depends_on: 
      - db
    build: ./backend
    ports: 
      - 3001:3001
    environment: 
      DB_URL: mongodb://db/vidly
    command: ./docker-entrypoint.sh

  db:
    image: mongo:4.0-xenial
    ports:
      - 27017:27017
    volumes:
      - vidly:/data/db

volumes:
  vidly:

```

----------------------

* docker compose is build in top of docker engine anything docker engine can do docker compose can do it 

```bash

docker compose

===================
Usage:  docker compose [OPTIONS] COMMAND

Docker Compose

Options:
      --ansi string                Control when to print ANSI control characters ("never"|"always"|"auto") (default "auto")
      --compatibility              Run compose in backward compatibility mode
      --env-file string            Specify an alternate environment file.
  -f, --file stringArray           Compose configuration files
      --profile stringArray        Specify a profile to enable
      --project-directory string   Specify an alternate working directory
                                   (default: the path of the Compose file)
  -p, --project-name string        Project name

Commands:
  build       Build or rebuild services
  convert     Converts the compose file to platforms canonical format
  cp          Copy files/folders between a service container and the local filesystem
  create      Creates containers for a service.
  down        Stop and remove containers, networks
  events      Receive real time events from containers.
  exec        Execute a command in a running container.
  images      List images used by the created containers
  kill        Force stop service containers.
  logs        View output from containers
  ls          List running compose projects
  pause       Pause services
  port        Print the public port for a port binding.
  ps          List containers
  # ***
  pull        Pull service images
  # ***
  push        Push service images
  # ***
  restart     Restart containers
  # ***
  rm          Removes stopped service containers
  # ***
  run         Run a one-off command on a service.
  start       Start services
  stop        Stop services
  top         Display the running processes
  unpause     Unpause services
  up          Create and start containers
  version     Show the Docker Compose version information

Run docker compose COMMAND --help for more information on a command.
===================

docker compose build --help

===================

Usage:  docker compose build [SERVICE...]

Build or rebuild services

Options:
      --build-arg stringArray   Set build-time variables for services.
      # ***
      --no-cache                Do not use cache when building the image
      --progress string         Set type of progress output (auto, tty, plain, quiet) (default "auto")
      # ***
      --pull                    Always attempt to pull a newer version of the image.
  -q, --quiet                   Dont print anything to STDOUT
===================





docker compose build

===================
.
.
.
===================


docker images

===================
# if you see the image is created an hour ago but you build the project now why do that because docker look at the image of no change then it is just previous one if change something it will change the time
REPOSITORY       TAG                  IMAGE ID       CREATED         SIZE
vidly_api        latest               17de95cb26d9   2 hours ago     184MB
vidly_backend    latest               17de95cb26d9   2 hours ago     184MB
vidly_frontend   latest               83c526da448a   2 hours ago     299MB
vidly_web        latest               83c526da448a   2 hours ago     299MB
mongo            4.0-xenial           d47c005e9fb1   4 weeks ago     430MB
node             14.16.0-alpine3.13   50bfd284aa0d   15 months ago   117MB
===================


# building with no cache
docker compose build --no-cache

```

------------------------

* running docker compose for my project in the background and stoping it

```bash

docker compose up --help

===================

Usage:  docker compose up [SERVICE...]

Create and start containers

Options:
      --abort-on-container-exit   Stops all containers if any container was stopped. Incompatible with -d
      --always-recreate-deps      Recreate dependent containers. Incompatible with --no-recreate.
      --attach stringArray        Attach to service output.
      --attach-dependencies       Attach to dependent containers.
      --build                     Build images before starting containers.
  -d, --detach                    Detached mode: Run containers in the background
      --exit-code-from string     Return the exit code of the selected service container. Implies --abort-on-container-exit
      --force-recreate            Recreate containers even if their configuration and image haven't changed.
      --no-build                  Don't build an image, even if it's missing.
      --no-color                  Produce monochrome output.
      --no-deps                   Don't start linked services.
      --no-log-prefix             Dont print prefix in logs.
      --no-recreate               If containers already exist, dont recreate them. Incompatible with --force-recreate.
      --no-start                  Dont start the services after creating them.
      --quiet-pull                Pull without printing progress information.
      --remove-orphans            Remove containers for services not defined in the Compose file.
  -V, --renew-anon-volumes        Recreate anonymous volumes instead of retrieving data from the previous containers.
      --scale scale               Scale SERVICE to NUM instances. Overrides the scale setting in the Compose file if present.
  -t, --timeout int               Use this timeout in seconds for container shutdown when attached or when containers are already running. (default 10)
      --wait                      Wait for services to be running|healthy. Implies detached mode.
===================


# by default it will build it is like you are type this ( docker compose up --build )
docker compose up


# if you want run this in background use -d in the end
docker compose up -d

docker compose ps

===================
NAME                COMMAND                  SERVICE             STATUS              PORTS
vidly-api-1         "docker-entrypoint.s…"   api                 running             0.0.0.0:3001->3001/tcp, :::3001->3001/tcp
vidly-backend-1     "docker-entrypoint.s…"   backend             exited (137)        
vidly-db-1          "docker-entrypoint.s…"   db                  running             0.0.0.0:27017->27017/tcp, :::27017->27017/tcp
vidly-frontend-1    "docker-entrypoint.s…"   frontend            exited (0)          
vidly-web-1         "docker-entrypoint.s…"   web                 running             0.0.0.0:3000->3000/tcp, :::3000->3000/tcp
===================

# Note: test your browser look at link: localhost:3000     link: localhost:3001

# down the project the image still run but when you down the docker compose for the project the free resource take back for my system 
docker compose down

docker ps

===================
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
===================


docker ps -a

===================
CONTAINER ID   IMAGE            COMMAND                  CREATED       STATUS                     PORTS     NAMES
8b3c6d713dce   vidly_frontend   "docker-entrypoint.s…"   2 hours ago   Exited (0) 2 hours ago               vidly-frontend-1
63733aac318b   vidly_backend    "docker-entrypoint.s…"   2 hours ago   Exited (137) 2 hours ago             vidly-backend-1
===================

```

----------------------

* creating network

```bash

docker compose up -d

===================
# in first line you are see that the docker is actually create network for this project that you run it
WARN[0000] Found orphan containers ([vidly-frontend-1 vidly-backend-1]) for this project. If you removed or renamed this service in your compose file, you can run this command with the --remove-orphans flag to clean it up. 
[+] Running 4/4
# this is the networ that is by default will be created
 ⠿ Network vidly_default      Created                                                                                                                                                                         0.1s
 ⠿ Container vidly-web-1      Started                                                                                                                                                                         1.0s
 ⠿ Container vidly-api-1      Started                                                                                                                                                                         1.0s
 ⠿ Container vidly-db-1       Started                                                                                                                                                                         1.0s
 ⠋ Container vidly-backend-1  Starting                                                                                                                                                                        0.0s
Error response from daemon: network 2c14c08b9669047926a7a7ac0ee20b587b8fa46fa96d173a18c904a7aa04da23 not found
===================


# show the network that is created 
docker network ls

===================
# every docker is have 3 network ( bridge host none )
NETWORK ID     NAME            DRIVER    SCOPE
4c18f2864a5c   bridge          bridge    local
6eaab29387cb   host            host      local
cd292fe3b125   none            null      local
# vidly is this network that is created   this is contain 3 host this is the ( web and api and db ) that we created it in docker-compose file
5225dd7943f2   vidly_default   bridge    local
===================


docker ps

===================
CONTAINER ID   IMAGE              COMMAND                  CREATED         STATUS         PORTS                                           NAMES
f5bf5645a3a7   mongo:4.0-xenial   "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp   vidly-db-1
aabceb8c68c2   vidly_api          "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   0.0.0.0:3001->3001/tcp, :::3001->3001/tcp       vidly-api-1
89887456674b   vidly_web          "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp       vidly-web-1
===================


# let's connect to one of the server and ping for it i use web for my self you can do one of them
docker exec -it -u root 89887456674b sh

===================
/app # ping web
PING web (172.19.0.4): 56 data bytes
64 bytes from 172.19.0.4: seq=0 ttl=64 time=0.077 ms
64 bytes from 172.19.0.4: seq=1 ttl=64 time=0.116 ms
64 bytes from 172.19.0.4: seq=2 ttl=64 time=0.106 ms
^C
--- web ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.077/0.099/0.116 ms
===================

# docker have DNS SERVER that containe the ip address of the each container 
# inside each container we have DNS RESOLVER this DNS RESOLVER talk with DNS SERVER to know what is the ip address of the ip address of own container after that the DNS SERVER response and send the ip address each container is part of the network


# to know the ip address of the container
ifconfig

===================
# look at first one means eth0
eth0      Link encap:Ethernet  HWaddr 02:42:AC:13:00:04
          # addr:172.19.0.4 is the ip address  
          inet addr:172.19.0.4  Bcast:172.19.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:101 errors:0 dropped:0 overruns:0 frame:0
          TX packets:4 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:13894 (13.5 KiB)  TX bytes:280 (280.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:14 errors:0 dropped:0 overruns:0 frame:0
          TX packets:14 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:987 (987.0 B)  TX bytes:987 (987.0 B)
===================

```

-------------------------------

* viweing log

```bash

docker compose logs --help

===================

Usage:  docker compose logs [SERVICE...]

View output from containers

Options:
  -f, --follow          Follow log output.
      --no-color        Produce monochrome output.
      --no-log-prefix   Dont print prefix in logs.
      --since string    Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
      --tail string     Number of lines to show from the end of the logs for each container. (default "all")
  -t, --timestamps      Show timestamps.
      --until string    Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
===================


# this will show you all logs for all this container that is running with docker compose
docker compose logs

===================
.
.
.
===================


docker ps

===================
CONTAINER ID   IMAGE              COMMAND                  CREATED          STATUS          PORTS                                           NAMES
f5bf5645a3a7   mongo:4.0-xenial   "docker-entrypoint.s…"   25 minutes ago   Up 25 minutes   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp   vidly-db-1
aabceb8c68c2   vidly_api          "docker-entrypoint.s…"   25 minutes ago   Up 25 minutes   0.0.0.0:3001->3001/tcp, :::3001->3001/tcp       vidly-api-1
89887456674b   vidly_web          "docker-entrypoint.s…"   25 minutes ago   Up 25 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp       vidly-web-1
===================

# syntax ( docker logs  -f [container_id] ) 
# for example you just want see the web logs real time
docker logs  -f 89887456674b

===================
.
.
.
===================

```

-----------------------

* read the change for changing real time 

```yml

version: "3.8"

services:
    web:
        build: ./frontend
        ports:
            - 3000:3000
    api:
        build: ./backend
        ports:
            - 3001:3001
        environment:
            DB_URL: mongodb://db/vidly
        # we add this line for real time changing for we do that for web interface section
        volumes:
          - ./backend:/app
    db:
        image: mongo:4.0-xenial
        ports:
            - 27017:27017
        volumes:
            - vidly:/data/db

volumes:
    vidly:

```

### Note:

* if you get error like not found this thing or something like that then try to install depedence for each backend frontend or try add this depedence for your project to not get this error again

* if you go to the browser and start the application and don't see anything then you should install depedencey this another sign that you have missed dependence

* install depedence

```bash

cd backend or frontend

npm install            or            npm i



# after that run the docker compose file again
docker compose up -d


# go to browser and look if you are see any change or wrong thing
# note: refresh the browser sometimes it does not do the change in real time

```

-------------------

* migrate mongo it the package that you can see the data in the backend versy easly and this data actully without this package can be see in software mongo like ( mongodb compass )

* to add this feature youshould first add this package into depedence in the backend like this

```json

// "migrate-mongo": "version...." 
"migrate-mongo": "^8.1.4",

```

* you should also have migrations directory that store migration start and stop

* to execute any migrate mongo script you should just go to terminal and type this:

```bash

# migrate is not excute multiple time just one time it is good we don't have overriding
migrate-mongo up      

or        
# this will run this package that is in depedence means in package.json
npm run db:up 

```

* to run this with docker compose the follow the step

```yml

version: "3.8"

services:
    web:
        build: ./frontend
        ports:
            - 3000:3000
    api:
        build: ./backend
        ports:
            - 3001:3001
        environment:
            DB_URL: mongodb://db/vidly
        volumes:
          - ./backend:/app
        ## we don't use this because the when we excute that the database may not be read the time when execute this command 
        ## this is why we use the wait script link-github: https://github.com/eficode/wait-for
        ## link for docker: https://docs.docker.com/engine/reference/commandline/wait/
        ## if you use the wait script then you should add to backend project after that add to the command
        
        ## do't do that
        # command: migrate-mongo up && npm start
        ## do that
        # command: ./wait-for db:27017 && migrate-mongo up && npm start

        ## after that you can use some of the beutify script in file name is ( docker-entrypoint.sh ) look at this file and see what is in that file
        ## if you do this script ( docker-entrypoing.sh ) then just type this

        command: ./docker-entrypoint.sh
    db:
        image: mongo:4.0-xenial
        ports:
            - 27017:27017
        volumes:
            - vidly:/data/db

volumes:
    vidly:

```

* file (docker-entrypoing.sh)

```bash

#!/bin/sh

echo "Waiting for MongoDB to start..."
./wait-for db:27017 

echo "Migrating the databse..."
npm run db:up 

echo "Starting the server..."
npm start 

```

* now start the docker compose

```bash

# first remove volume (vidly_vidly) for my project this volume is for my database
docker volume ls

===================
DRIVER    VOLUME NAME
local     1293fd1fd8d51da76eb7b5f58b09e3a81f9c7018bad00e2bb5fc99a30eee1b5b
local     app-data
local     e950abd8d39ca7b2af3c2a30c2662bc41447ede2f006585825dd09c3b5a2a4b2
local     pwd
local     vidly_vidly
===================

docker volume rm vidly_vidly

===================
vidly_vidly
===================

docker compose up -d

# now test for backend go to this link: http://localhost:3001/api/movies 
# if you see json output then it is work
# now test for frontend go to this link: http://localhost:3000/
```

----------------------

* running test

```bash

# go to this folder that you want run the test first
cd frontend            or               cd backend

# to run the test with jsone you can type
npm test

```

```yml

version: "3.8"

services:
    web:
        build: ./frontend
        ports:
            - 3000:3000
        # this volume i forget i type it before
        volumes:
            - ./frontend:/app
    # i create another section inside service to manage test and test for front end
    web-tests:
        # i delete the build for the ./frontend change it to the image i test for image that have the project
        image: vidly_web
        # i used the volume that i create it for the frontend inside this in the image it test
        volumes:
            - ./frontend:/app
        # end the end it should run this command npm start to start the test
        command: npm test
    api:
        build: ./backend
        ports:
            - 3001:3001
        environment:
            DB_URL: mongodb://db/vidly
        volumes:
          - ./backend:/app
        command: ./docker-entrypoint.sh
    # also you can do that for api means backend just create section with different name
    api-test:
        # this is the image name for backend
        image: vidly_api
        # i use this volume in the image
        volumes:
          - ./backend:/app
        # start this command
        command: npm start
    db:
        image: mongo:4.0-xenial
        ports:
            - 27017:27017
        volumes:
            - vidly:/data/db

volumes:
    vidly:

```
