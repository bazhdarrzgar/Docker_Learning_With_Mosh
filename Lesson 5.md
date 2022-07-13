## Different between container and images

* image

    - A cut-down os
    - Third-party libraries
    - Application files
    - Environment variables

* container

    - Provides an isolated environment
    - Can be stopped & restarted
    - Is just a process!


* create different container in one container

```bash

docker start -i 1c6326fc5ba8

===================
root@1c6326fc5ba8:~# 
===================

exit

docker run -it ubuntu

===================
# this will start with different id
root@f57b4c192a30:/# 
===================

exit

docker ps -a

===================
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS                          PORTS     NAMES
f57b4c192a30   ubuntu      "bash"                   40 seconds ago   Exited (0) 3 seconds ago                  great_proskuriakova
fb0114f72029   react-app   "docker-entrypoint.s…"   7 hours ago      Exited (0) 7 hours ago                    romantic_swanson
1c6326fc5ba8   ubuntu      "bash"                   9 hours ago      Exited (0) About a minute ago             blissful_galileo
2acfe607b1b6   ubuntu      "bash"                   9 hours ago      Exited (0) About a minute ago             focused_brattain
===================

-------------------------

* work with docker

```bash

# for specify the base image means we have some file and directory and run all this in top of the image
FROM

# specify the working directory means this directory that all operation will operate on it
WORKDIR

# Copy something like file or folder
COPY

# Adding something like file or folder or ....
ADD

# excuting all this command that above will run using run command
RUN

# setting environment variable
ENV

# container start in port what ..
EXPOSE

# run application with limited priviledge
USER

# this command that should be excuting when container start
CMD
ENTRYPOINT

```

-----------------------------------

* Run project but we run it with docker

    - More info: https://docs.docker.com/samples/dotnetcore/

* example

```dockerfile

# syntax=docker/dockerfile:1
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY ../engine/examples ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "aspnetapp.dll"]

```

* to search for image in docker website go to docker hub

    - link: https://hub.docker.com/

* for example i want node go this url

* if you go to the tags tab you will see there is a lot of node version

* also if you look at any of the node version you will see the node is run in top of different operating system linux why not windows because windows is fucking large

* note alpine image is very small if very lite good choice

    - link: https://hub.docker.com/_/node?tab=tags

```dockerfile

# means download the latest version of node
# # FROM node:latest
# if you want some thing smaller than this you should specify the operating system
# this will be install the latest version of node and run it in top of the alpine
FROM node:lts-alpine3.16

```

* now build and run the project

```bash

# Note: this project will run inside directory of the project it self
# build the project in 
docker build -t react-app .

# if you run image like this, you will directory go to the node termina
docker run -it react-app

# if you want go to the bash termina or sh sometimes os is have one of them
docker run -it react-app sh             or            docker start -i 051c4230deb4 

# now if you are in shell or bash look if node install
node --version

===================
v16.16.0
===================

```


--------------------

* adding some feature to the file

```dockerfile

FROM node:lts-alpine3.16
WORKDIR /app
# coping all file and directory from current directory because we build the image in current directory (docker build -t react-app . ) now the docker will copy all this in current directory now we are working on this will be /app directory if this directory is not exist then it will create it
COPY . .

```

* build it

```bash

docker build -t react-app .

===================
Sending build context to Docker daemon  866.8kB
Step 1/3 : FROM node:lts-alpine3.16
 ---> b48aa2e90c0a
Step 2/3 : WORKDIR /app
 ---> Using cache
 ---> d91d0529d8ac
Step 3/3 : COPY . .
 # this will using cache this is why it is faster because previously i run it
 ---> Using cache
 ---> 8dada96d0339
Successfully built 8dada96d0339
Successfully tagged react-app:latest
===================

```

* start it

```bash

docker run -it react-app sh             or            docker start -i 051c4230deb4 

===================
/app # 
/app # ls
README.md     dockerfile    package.json  public        src           yarn.lock
===================

```

------------------------

* reducing the size of the build when it is build for more effesent build

```bash

# normal build
docker build -t react-app .

```

* build using ( .dockerignore )

```dockerignore

# create file in your project and name is ( .dockerignore )
# this will ignore folder node_modules when build the project
node_modules/ 

```

------------------------

* adding more feature for dockerfile

```dockerfile

FROM node:lts-alpine3.16
WORKDIR /app
COPY . .
# when the image is boot this command will be run
RUN npm install

```

* run it and build it

```bash

# build it
docker build -t react-app .

# run it
docker run -it react-app sh             or            docker start -i 051c4230deb4 

```
---------------------

* environment variable

```dockerfile

FROM node:lts-alpine3.16
WORKDIR /app
COPY . .
RUN npm install


# adding environment variable

ENV API_URL=http://api.myapp.com/
# ENV API_URL http://api.myapp.com/     # this is also true

```

* run it and build it

```bash

# build it
docker build -t react-app .

# run it
docker run -it react-app sh             or            docker start -i 051c4230deb4 

```

* check if change is made

```bash

printenv API_URL

===================
http://api.myapp.com/
===================

# or you can print like this
echo $API_URL

===================
http://api.myapp.com/
===================

```

-------------------------

* work with port

```dockerfile

FROM node:lts-alpine3.16
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.myapp.com/

# the react app in this project run in port 3000 to talk with docker that i want run this porject with port 3000 and don't use this port for other work you should use EXPOSE
EXPOSE 3000

```

-------------------------

* setting user

```bash

docker run -it alpine



# more info about adduser
adduser

===================

BusyBox v1.35.0 (2022-05-09 17:27:12 UTC) multi-call binary.

Usage: adduser [OPTIONS] USER [GROUP]

Create new user, or add USER to GROUP

	-h DIR		Home directory
	-g GECOS	GECOS field
	-s SHELL	Login shell
    # we need create group also
	-G GRP		Group
    # we need create system user
	-S		Create a system user
	-D		Don't assign a password
	-H		Don't create home directory
	-u UID		User id
	-k SKEL		Skeleton directory (/etc/skel)

===================


# create group
addgroup app

groupapp

===================
app
===================




# adding user for this group that i want
adduser -S -G [name_group] [name_user_you_want_create]

# example
adduser -S -G app alex



# you can do both of this like this:
addgroup app && adduser -S -G app alex


```

* put into docker file

```dockerfile

FROM node:lts-alpine3.16
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.myapp.com/
EXPOSE 3000

## now i put the command in run command section
RUN addgroup app && adduser -S -G app alex

## now specify the USER
USER alex

```

* run it and build it

```bash

# build it
docker build -t react-app .

```

* looking at privilege of this user

```bash

docker run -it react-app sh

whoami

===================
# now any one start the application it should see the alex user
alex
===================

ls -l

===================

===================

```

-----------------------------------

* copy file and folder form docker to local machine

```bash

# create new container
docker run -d --name container-name image-name

# first you need name the container that you want copy file or folder form it
docker ps -a

===================
# in end column you can see the name of the container you need this name
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS                     PORTS      NAMES
490597c2d729   react-app      "docker-entrypoint.s…"   46 seconds ago   Up 46 seconds              3000/tcp   upbeat_banzai
9a06d1ad717c   18e7508e5944   "docker-entrypoint.s…"   14 minutes ago   Exited (0) 2 minutes ago              elastic_northcutt
===================

# run it yo can run like me as bash
# docker exec -it docker-name /bin/bash
docker exec -it cupbeat_banzai /bin/bash


# find the place and folder and copy the path and after that exit to the local host

# sudo docker cp docker-name:/file.txt ~/Desktop/file_copy.txt
sudo docker cp upbeat_banzai:/file.txt ~/Desktop/file_copy.txt

## some note and demo:

* docker should be run if not run this is not doing work

docker start -i cdd9a81c5789

* now you can copy anything in your container to you local machine

docker cp focused_germain:/app/node_modules /home/soyansoon/Documents/Trash/l

```

-----------------------------------

* let's understand more about ( docker run react-app .... )

```bash

# start docker with starting npm means after name of the start container the command can be excute
docker run react-app npm start
docker run react-app sh

# if you run this you will see this error

===================
EACCES: permission denied, mkdir '/app/node_modules/.cache'
===================

```

* to fix this you should change the 

```dockerfile
# now error will be fix after modifying this two line in the end to move it after FROM because user should create first of every thing
FROM node:lts-alpine3.16
RUN addgroup app && adduser -S -G app alex
USER alex
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.myapp.com/
EXPOSE 3000

```

* you can start the npm start like this in the dockerfile

```dockerfile

FROM node:lts-alpine3.16
RUN addgroup app && adduser -S -G app alex
USER alex
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.myapp.com/
EXPOSE 3000

# different between CMD and RUN 
# the cmd is normal running command not doing anything just run it command
# the RUN will run the command with any change that is big like build the image means run the excution this build we do when docker is build
CMD npm start

## don't use this way
# CMD ["npm", "start"]

## this is in some way is different from CMD how 
# ENTRYPOINT  ["npm", "start"]

```


------------------------------

* some note

    - any time you change to the docker file and you want see the change when run the docker you should build it again
    - use .dockerignore for ignore this file and folder you want to copy

-------------------------------

* problem with runnig with ( npm install ) fix

```bash

# first solution install npm mroe info ( https://www.npmjs.com/package/npm )
npm i npm


# install exactly version when error take you more info ( https://stackoverflow.com/questions/72442682/npm-err-code-err-socket-timeout-npm-err-network-socket-timeout/72502205 )
npm i -g npm@8.5.1


# if this is not error you are socket error not fix ( “npm ERR! code ERR_SOCKET_TIMEOUT” Code Answer’s )
# do this 
# link: https://www.codegrepper.com/code-examples/whatever/npm+ERR%21+code+ERR_SOCKET_TIMEOUT

$ npm config rm proxy
$ npm config rm https-proxy

$ npm config rm proxy
$ npm config rm https-proxy

* Your command cannot succeed, because npm fails to fetch some module due to internet connection problems. You can try using different internet connection or increasing npm fetch timeouts.
* I just had this issue and it helped changing timeouts:

npm config set fetch-retry-mintimeout 20000
npm config set fetch-retry-maxtimeout 120000

```

* error copy use this link stack over flow

    - link: https://stackoverflow.com/questions/64613524/docker-cp-throws-no-such-containerpath-on-running-instance

```bash

$ docker run -d --name httpd httpd

$ docker exec httpd pwd
/usr/local/apache2

$ docker exec httpd ls
bin
build
cgi-bin
conf
error
htdocs
icons
include
logs
modules

$ docker cp httpd:/usr/local/apache2/conf .

```

* if none of this work try this:

```bash

npm install
npm install https://github.com/npm/cli/releases/tag/v8.13.2
npm install -g npm@8.13.2
npm i npm

````


