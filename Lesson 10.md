* installing docker machine

    - we use docker machine to talk with docker engine to response it when we deploy your website you need docker machine for request to the docker engine for that we use docker machine
    
    - this command that we execute it with docker machine it will be send to docker engine to execute this command and run it

    - link: https://github.com/docker/machine/releases/tag/v0.16.2

```bash

# on linux
 curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine &&
    chmod +x /tmp/docker-machine &&
    sudo cp /tmp/docker-machine /usr/local/bin/docker-machine



# know the version

docker-machine --version

===
docker-machine version 0.16.2, build bd45ab13
===



docker machine --version

===
Docker version 20.10.17, build 100c701
===

```

---------------------------

* create digitalocean server for us

    - docker machine driver
        
        + link: https://docker-docs.netlify.app/machine/drivers/
    
    - create a toke for your self in digitalocean website
        
        + link: https://cloud.digitalocean.com/account/api/tokens?!=d5c9b3

    - unable to verify the docker daemon is listening:

        + link: https://www.digitalocean.com/community/questions/unable-to-verify-the-docker-daemon-is-listening

```bash

# NOTE:
## in place of $DOTOKEN you should past this token that generated by digitalocean website
## i added engine-install-url this is problem it should be fixed by docker machine but for know it is work
# name can be any name 
# by default the os for your server will be ubuntu to chnage it go to this website: https://docker-docs.netlify.app/machine/drivers/digital-ocean/   look at the option you see ( --digitalocean-image: The name of the Digital Ocean image to use )

# syntax:
# docker-machine create \
        # --driver digitalocean \
        # --digitalocean-access-token $DOTOKEN \
        # --engine-install-url "https://releases.rancher.com/install-docker/19.03.9.sh" \
        # name; 

# practic
docker-machine create \
        --driver digitalocean \
        --digitalocean-access-token llk23jllj2lktlk2j3lkrj23lk4j23ljoiawejofuw4ijti4wjtowiejf \
        --engine-install-url "https://releases.rancher.com/install-docker/19.03.9.sh" \
        vidly; 


# in the end you should see in the website digitalocean that your web server is ready with less storage and ram and take you ip for look at your live server you created but this is not start yet for starting go to the next lesson

```

-------------------------

* connect to the server

```bash

docker-machine ls

===
NAME   ACTIVE   DRIVER        STATE     URL                       SWARM   DOCKER    ERRORS
vidly  -        digitalocean  Running   tcp://104.131.24.150:237          v19.03.9
===

 
# connect to the server with ssh ( secure shell )
docker-machine ssh vidly

===
.
.
.
root@vidly:~# ls

root@vidly:~# cd /

root@vidly:~# ls
# output will be file and folder system .........
root@vidly:~# 


===

```

---------------------------

* production of the project

    - when you production your project you don't need to use some of the line of the docke-compose like testing or volume this is just usefull when you are build your project you should delete this line to not slow down the server
    
    - edit the docker-compose.yml

    - info about restart property link: https://docs.docker.com/compose/compose-file/compose-file-v3/#restart

```yml

# ( no ) will not restart the container when crashes
restart: "no"
# ( always ) always restart the container
restart: always
# ( on-failure ) restart the container when crash the container
restart: on-failure
# ( unless-stopped ) restart the container when it is stop
restart: unless-stopped

```

```yml

version: "3.8"

services:
    web:
        build: ./frontend
        # change the port for something else this is better when you production your project port 80 is port use by google or localhost
        ports:
            - 80:3000
        # removing volume for the web for more info about restart go to this link: https://docs.docker.com/compose/compose-file/compose-file-v3/#restart
        restart: unless-stopped
    # removing the test for web
    api:
        build: ./backend
        # for the backend no matter what port you are using
        ports:
            - 3001:3001
        environment:
            DB_URL: mongodb://db/vidly
        # removing volume for the api
        command: ./docker-entrypoint.sh
        # when container stop will restart the container
        restart: unless-stopped
    # removing the api test 
    db:
        image: mongo:4.0-xenial
        ports:
            - 27017:27017
        volumes:
            - vidly:/data/db
        # when container stop will restart the container
        restart: unless-stopped
volumes:
    vidly:

```

-------------------------------

* reducing the image size

```bash

# if you look at the vidly_api and vidly_web is nearly 300MB then how we reduce the image size
docker images

===
REPOSITORY       TAG                  IMAGE ID       CREATED         SIZE
vidly_api        latest               17de95cb26d9   18 hours ago    184MB
vidly_backend    latest               17de95cb26d9   18 hours ago    184MB
vidly_web        latest               83c526da448a   18 hours ago    299MB
vidly_frontend   latest               83c526da448a   18 hours ago    299MB
mongo            4.0-xenial           d47c005e9fb1   4 weeks ago     430MB
node             14.16.0-alpine3.13   50bfd284aa0d   15 months ago   117MB
===


# in build folder you get all the asset for deploy your application
cd frontend

# creating optimize asset for the production
npm run build

===
.
.
.
===

```

* now let's code some of the file

1. create file name is Dockerfile.prod done this code inside it this is done in frontend application

```dockerfile

# Step 1: Build stage 
# ( AS build-stage ) is command to name this layer
FROM node:14.16.0-alpine3.13 AS build-stage
WORKDIR /app
COPY package*.json ./
COPY . . 
RUN npm install
CMD ["npm", "start"]

# nginx is just web server we choose this because is light way and we run in the top of the alpine linux
# Step 2: Production
FROM nginx:1.12-alpine
RUN addgroup app && adduser -S -G app app
USER app
# this done from build-stage means this place that have all this thing
# we copy all component build depedence from (/app/build) to (/share/nginx/html) to the web server
COPY --from=build-stage /app/build /usr/share/nginx/html
# port 80 is default port for transaction
EXPOSE 80 
# every time i want build this project i want this command be excute it
ENTRYPOINT [ "nginx", "-g", "daemon off;" ]

```

2. run this 

```bash

# new image is name is (vidly_web_opt)
docker build -t vidly_web_opt -f Dockerfile.prod .

docker images

===
....
....
# (vidly_web_opt) images it should be reduce size very mache like from 300MB to 16.2MB
===

```

3. back to docker compose file create another file name is docker-compose.prod.yml and do this staf on it

```yml

version: "3.8"

services:
    web:
        build: ./frontend
            contex: ./frontend
            # specify the dockerfile
            dockerfile: Dockerfile.prod
        # change the port for 80 by 80 because the transcation will be between 80 by 80 in the interenet and server
        ports:
            - 80:80
        restart: unless-stopped
    api:
        build: ./backend
        ports:
            - 3001:3001
        environment:
            DB_URL: mongodb://db/vidly
        command: ./docker-entrypoint.sh
        restart: unless-stopped
    db:
        image: mongo:4.0-xenial
        ports:
            - 27017:27017
        volumes:
            - vidly:/data/db
        restart: unless-stopped
volumes:
    vidly:

```

4. go to bash and build the docker compose file but new docker compose file not development production

```bash

docker compose -f docker-compose.prod.yml build

===
.
.
.
.
===


docker images 

===
# look at (vidly_web) and see if the size of this image also reduce from 300MB to 16.2MB
===

```

------------------------------

* Deploying the application


```bash

# information about this server that run for your project in the internet the info is very basic like ( name the server ) ( active or not ) ( driver ) ( state running ro not ) ( url for my website )
docker-machine ls

# this take you this environment variable that you want for talk with machine it take you this environment variable that you should set it in order to communication between you and docker machine is set
docker-machine env vidly

===
# you need just set this environment varialbe this will set all this environmet variable that you want you don't need set other environment variable just use this 
# this will show in the end of the output
eval $(docker-machine env vidly)
===

eval $(docker-machine env vidly)

docker images

docker-compose -f docker-compose.prod.yml up -d

```

* if in the build you get error the you should edit the Dockerfile for the frontend like this

    - NOTE: this problem is we have with digitalocean or may be just problem with docker version it should be fix now

    - first go to the backend ( cd backend )

```dockerfile

FROM node:14.16.0-alpine3.13

RUN addgroup app && adduser -S -G app app
# add this line to fix your problem
# this will create directory /app and take permision for group uapp to get any access to this folder
RUN mkdir /app && chown app:app /app
USER app

WORKDIR /app
COPY package*.json ./
COPY . .
RUN npm install

EXPOSE 3000 

CMD ["npm", "start"]

```

* now build it again

```bash

docker-compose -f docker-compose.prod.yml up -d --build

```

--------------------------------

* fix problem

* NOTE: if you test the application by going to target ip you get this ip by using ( docker-machine ls ) you get in column URL just copy the ip not the port if work is work if not work check the backend if this is work then then the problem in frontend 

* let's fix error

```bash

# go to see the image fron the container if the container vidly_web is restarting then it should have problem
docker ps

===
# in the columne time you should see restarting for the vidly_web container
===


# watch the logs of this container to see the message error
docker logs [container_id_for_(vidly_web)]

===
# there is a lot of error but all error is the same because the server is restart all time this will happen every message is the same
# the error is just about permission denied and this dockefile for the fronend it should run as root user for dockerfile
===

```

* go to frontend project and edit the Dockerfile.prod

```dockerfile

FROM node:14.16.0-alpine3.13 AS build-stage
WORKDIR /app
COPY package*.json ./
COPY . . 
RUN npm install
CMD ["npm", "start"]

FROM nginx:1.12-alpine
## remove this two line and problem should be fix
# RUN addgroup app && adduser -S -G app app
# USER app
COPY --from=build-stage /app/build /usr/share/nginx/html
EXPOSE 80 
ENTRYPOINT [ "nginx", "-g", "daemon off;" ]

```

* now again build the image


```bash

docker-compose -f docker-compose.prod.yml up --build

# look at the image if this is running means not restart
docker ps 

```

* you should still get the error because you should set the environment variable

* go to frontend project and edit the Dockerfile.prod

```dockerfile

FROM node:14.16.0-alpine3.13 AS build-stage
WORKDIR /app
COPY package*.json ./
COPY . . 


## add environment variable for the backend
## styntax
## ENV REACT_APP_API_URL=http://[url-of-your-website]:3001/api

ENV REACT_APP_API_URL=http://104.131.24.150:3001/api
RUN npm install
CMD ["npm", "start"]

FROM nginx:1.12-alpine
COPY --from=build-stage /app/build /usr/share/nginx/html
EXPOSE 80 
ENTRYPOINT [ "nginx", "-g", "daemon off;" ]

```

* the all problem should be gone if you still have this error follow this step

```bash

# active the server if not active you can watch this by using
# the ACTIVE column should take you start * for the active
docker-machine ls


# not forget to set environemt varialbe
docker-machine env vidly

===
# use just end the output command
eval $(docker-machine env vidly)
===

# end the end
docker compose up

```

-----------------------

* publish the change

```bash

docker ps

===
# you should see your container that is running but you should be aware that this image is just name you should take version name for it to know the user what version of the image you are using or what version of the application acually using for the docker 
===

```
* this is why we should edit the docker compose for naming the image

```yml

version: "3.8"

services:
    web:
        build: ./frontend
            contex: ./frontend
            dockerfile: Dockerfile.prod
        # name the image for web
        image: vidly_web:1
        ports:
            - 80:80
        restart: unless-stopped
    api:
        build: ./backend
        # name the image for api also
        image: vidly_api:1
        ports:
            - 3001:3001
        environment:
            DB_URL: mongodb://db/vidly
        command: ./docker-entrypoint.sh
        restart: unless-stopped
    db:
        image: mongo:4.0-xenial
        ports:
            - 27017:27017
        volumes:
            - vidly:/data/db
        restart: unless-stopped
volumes:
    vidly:

```


```bash

docker compose -f docker-compose.prod.yml -d --build

# to see the change for image name
docker ps 

```

* NOTE: that any time you want change the image name you should just use property of ( image: image_name ) to docker-compose file after that build it again in the background and see the change