* copy and past

```bash

docker ps

===
CONTAINER ID   IMAGE       COMMAND                  CREATED             STATUS             PORTS                                       NAMES
9670469c099b   react-app   "docker-entrypoint.s…"   About an hour ago   Up About an hour   0.0.0.0:5000->3000/tcp, :::5000->3000/tcp   condescending_gauss
19ee9a1d2e59   react-app   "docker-entrypoint.s…"   11 hours ago        Up 2 hours         3000/tcp                                    silly_moore
4d15b4bc626a   react-app   "docker-entrypoint.s…"   12 hours ago        Up 2 hours         0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
===



docker exec -it 9670469c099b sh

===
/app # echo hello > file.txt
/app # ls
Dockerfile         README.md          data               file.txt           node_modules       package-lock.json  package.json       public             src                yarn.lock
/app # cat file.txt
hello
/app # exit
===


# now let's copy the file from the docker container 9670469c099b to the host
# docker cp source_id:path destination_path

# . means this path that i currently i have in
docker cp 9670469c099b:/app/file.txt .

or 

docker cp 9670469c099b:/app/file.txt /home/soyansoon/Documents/Trash


ls

===
Dockerfile  file.txt  package.json  package-lock.json  public  README.md  src  yarn.lock
===

cat file.txt

===
hello
===





# let's copy file from the host to the container
echo hello > file_host.txt

cat file_host.txt

===
hello
===



# docker cp path_file_in_host destination_container_id:destination_path

docker cp file_host.txt 9670469c099b:/app

docker exec -it 9670469c099b sh

===
/app # ls
Dockerfile         README.md          data               file.txt           file_host.txt      node_modules       package-lock.json  package.json       public             src                yarn.lock
/app # cat file_host.txt
hello
/app # exit
===

```


-------------------------------

* real time changing

```bash

# build it in the beganning with this file

===
#### Dockerfile ####
FROM node:14.16.0-alpine3.13
# RUN addgroup app && adduser -S -G app app
# USER app
WORKDIR /app
# RUN mkdir data
# COPY package*.json .
COPY . . 
RUN npm install
ENV API_URL=http://api.myapp.com/
EXPOSE 3000
CMD ["npm", "start"]
===

docker build -t react-app .

# NOTE: if depedencey for node module is fail manully for your self do that means copy file and folder from your host to the docker container






# first let's test some of the technique of the volume to now if work
docker volume create app-data

===
app-data
===

docker volume inspect app-data

===
[
    {
        "CreatedAt": "2022-07-12T12:10:52+03:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/app-data/_data",
        "Name": "app-data",
        "Options": {},
        "Scope": "local"
    }
]
===

docker run -d -p 4000:3000 -v app-data:/app/data react-app

===
7c14b9ed1a0499b63929a8985f137e5127fd3fc5ec684a2f09507fcc9f16a956
===


docker ps

===
CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS         PORTS                                       NAMES
cb4ca66129ff   react-app   "docker-entrypoint.s…"   9 seconds ago   Up 8 seconds   0.0.0.0:4000->3000/tcp, :::4000->3000/tcp   strange_keller
===

docker container rm -f cb4ca66129ff

===
cb4ca66129ff
===







# to do that we should put all the file in the project to the directory in the container that my project is in
# i use pwd to now i am in this directory that my project currently on it
# /app because i want the be the copy is in the /app directory
# react-app is my image name
# if your path have folder that have two name and sprated by space you get this error

pwd

===
/home/soyansoon/Documents/Study/My_Course/Docker Course/code/react_2
===



docker run -d -p 5001:3000 -v $(pwd):/app react-app

===
docker: invalid reference format: repository name must be lowercase.
See 'docker run --help'.
===


docker images

===
REPOSITORY   TAG                  IMAGE ID       CREATED          SIZE
react-app    latest               90ed31added3   22 minutes ago   295MB
node         14.16.0-alpine3.13   50bfd284aa0d   15 months ago    117MB
===




# solving by just putting this two name folder into single quotest or you can use double quotes
docker run -d -p 8001:3000 -v /home/soyansoon/Documents/Study/My_Course/'Docker Course'/code/react-app:/app -it react-app

===
fb2ef15eb07b580546943cbf39419b7e86766d1de307075a6c5f0dbf1401cceb
===


docker ps

===
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS          PORTS                                       NAMES
fb2ef15eb07b   react-app     "docker-entrypoint.s…"   2 seconds ago    Up 1 second     0.0.0.0:8001->3000/tcp, :::8001->3000/tcp   funny_austin
87ac47aced91   react-app     "docker-entrypoint.s…"   53 seconds ago   Up 52 seconds   0.0.0.0:8000->3000/tcp, :::8000->3000/tcp   practical_lumiere
===




docker logs -f fb2ef15eb07b

===
Compiled successfully!

You can now view react-app in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://172.17.0.3:3000

Note that the development build is not optimized.
To create a production build, use yarn build.

===




# now go to the localhost port 8001 link: ( localhost:8001 ) it should be work
# now make any change if you want do that the change back to you in real time

```

* basic sharing folder and file from host to contianer

```bash

docker run -d -v $(pwd):/app -it hello-world

```

* some note take it

```bash

# use this command to make sure your container is running the application and not have error with running other wise the port not work
docker exec -it [container_id]

# build the image with full detail in the dockerfile
docker build -t [name] [path_your_application]

# use this command if the port is point the port in the container 
docker ps

# make sure the container run in the background use this command for that
docker ps

```