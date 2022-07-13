
## Some Of Bash Trick

```bash

CTRL + W ( remove enter word in terminal )
CTRL + TAB ( complete the word for you )
CTRL + D ( if CTRL + C is not terminate the command then CTRL + D is complete it to run this )


ls -1 ( sort it but in up to down like ls -l not like default ls )


mkdir test
ls
===
test
===

# rename it for that we use mv it can be used for file or folder
mv test docker-folder
ls
===
docker-folder
===





# if file very long you can use more command to view the content of file more visualization
more /etc/adduser.conf

====
..
..
..
--More-- (%15)
# press q for exit
# press space for go to the next page or enter one line at a time
# note: you can scroll up for that use less command
===

# using less command to replace more and fix scroll up in more
less /etc/adduser.conf

===
..
..
..
--More-- (%15)
# just use up and down arrow
# use space to go to the next page
# you can steal use enter for going to the next page
===






head -n 5 /etc/adduser.conf

===
# vewing first 5 line in up
.
.
.
.
.
===

tail -n 5 /etc/adduser.conf

===
# vewing last 5 line in down
.
.
.
.
.
===






# printing both of the line to the terminal ( prompt )
cat file1 file2

===
hello world
hello world
world hello
world hello
===

# combine both of this file into one file
cat file1 file2 > file3

===
hello world
hello world
world hello
world hello
===




# print long list in a file
ls -l /etc > files.txt


```

-------------------------------------------------------------

## some note

```bash

docker create # create container
dokcer exec # to run commnads in container for once
docker volume # create a docker volume
docker network # create a docker network
docker rm # remove container 
docker images # list the images
docker rmi # remove image
docker build # build a new image from dockerfile
docker push # push your image to docker repo
docker pull # download an image from docker repo
docker commit # create an image from container

```

-------------------------------------------------------------

## About Docker

* install docker

	-Linke: https://docs.docker.com/engine/install/

```bash

# knowing the version
docker --version

===
# about the version docker
..
..
..
===




# run any docker image by using

docker run [name the image]

```

* creating docker file small project

	- Name file: app.js

```js

console.log("hello world");

```

	- Name file: Dockerfile

```dockerfile

FROM node:alpine
COPY . /app
WORKDIR /app
CMD node app.js

```

	- in bash do this staf for running your project with docker

```bash

# bulild my project
docker build -t hello-docker .

# see the image
docker image ls                   or                   docker images

# run the project
docker run hello-docker

```

* Some Note:

	- public your repository to the world by creating repository docker website
		
		+ link: https://hub.docker.com/repositories

	- go to this link to play with docker 
		
		+ link: https://labs.play-with-docker.com/


* seeing the list of this container that running

	docker ps -a

or

	docker ps


* if there is not container run for example this is ubuntu and need run with container you should run it like this:

docker run -it ubuntu

---------------------------------------------------

## Docker Container

```bash

# see the images on my machine
docker images

# see this images that have container
docker ps

# see the including depedence for the container
docker ps -a

# remove images
docker image rm image-id-1 image-id-2 ...

# see the image id like this
docker image ls -q

# see all the image with more information along with id
docker image ls

# remove all the images
docker image rm -f $(docker image ls -q)


# remove the container
# NOTE: -a for stoping the container
docker container rm -f $(docker container ls -a -q )  

```


* to restart docker from the engine

1. Docker Compose Software
2. go to debuging tab
3. clean / purge data

* Note: now docker engine will be restart and all the image and container will be remove


* how to create docker compose file
	
	- default name for docker compose is ( docker-compose.yml )

* how to run this file

```yml

docker compose up

```

* different between yml language and json

	- in json how we code

```json

{
	"name": "The Ultimate Docker Course",
	"price: 149,
	"is_published": true,
	"tags": ["software", "devops"],
	"author": {
		"first_name": "Mosh",
		"last_name": "Hamedani"
	}
}

```

	- in yml code

```yml

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

* **Note**: yml is easy to read and write but it is slower than json

* Creating Docker Compose File

	- Name file: docker-compose.yml

```yml

version: "3.8"

services:
	web:
		build: ./frontend
		ports:
			- 3000: 3000
	api:
		build: ./backend
		ports: 
			- 3001:3001
		environment:
			DB_URL: mongodb://db/vidly
	db:
		image: mongo:4.0-xenial
		ports:
			- 27017:27017
		volumes:
			- vidly:/data/db

volumes:
	vidly:

```

```bash

# more detail about docker compose
docker compose

# more detail about docker compose build
docker compose build --help
# import detail in this list is:
	--no-cache # using no cache when running the image
	--pull # pulling newest version of the image

```

```bash

# now we run the project
docker compose build

# see the image
docker images

# if you have running this file before the second time you run it the depedence will not install again it will just look if exist or no and if it is the new thing it will be install it
# if you want not do this docker for you use --no--cache
docker compose build --no-cache

```

```bash

# turn of the all container
docker compose down

# start my container
docker compose up -d

# if the docker is turn off and also the container for this project that you have also turn off to run it you should use 

docker compose up 

# more detail about docker compose up 
docker compose up --help
# important detail about this is:
--build # Build images before starting container 
-d # for detash mod this will start the container in the background
	- for more detail about this to know if this is work type
		
		docker compose ps

```

* docker with network

```bash

# looking at the network
docker network ls

# looking at the running container
docker ps

# ping for the api
docker exec -it -u root [first-3-character of the container id this will detect by using ( docker ps ) ] sh

ping api

===
.
.
.
===

# looking at the ip address of the container
ifconfig
```

--------------------------------------------