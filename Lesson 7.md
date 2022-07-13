* save image (means the docker image) and load it into another machine

```bash

docker image --help

===================
Usage:  docker image COMMAND

Manage images

Commands:
  build       Build an image from a Dockerfile
  history     Show the history of an image
  import      Import the contents from a tarball to create a filesystem image
  inspect     Display detailed information on one or more images
  load        Load an image from a tar archive or STDIN
  ls          List images
  prune       Remove unused images
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rm          Remove one or more images
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

Run 'docker image COMMAND --help' for more information on a command.
===================





docker image save --help

===================
Usage:  docker image save [OPTIONS] IMAGE [IMAGE...]

Save one or more images to a tar archive (streamed to STDOUT by default)

Options:
  -o, --output string   Write to a file, instead of STDOUT
===================
 




docker image save -o react-app.tar react-app:3

===================
dockerfile  package.json  public  react-app.tar  README.md  src  yarn.lock
===================

# NOTE: this tar is contain all layer of your image the image is divided is some folder each folder is have information about ther layer for example there is folder for your os and there is folder for your node and also folder for your application and ...






docker image load --help

===================
Usage:  docker image load [OPTIONS]

Load an image from a tar archive or STDIN

Options:
  -i, --input string   Read from tar archive file, instead of STDIN
  -q, --quiet          Suppress the load output
===================






# now let's delete the image and load it again to the docker

docker images

===================
REPOSITORY                       TAG              IMAGE ID       CREATED       SIZE
829282/hello-docker-repository   5                2b579ddfacdc   5 hours ago   113MB
react-app                        4                2b579ddfacdc   5 hours ago   113MB
react-app                        5                2b579ddfacdc   5 hours ago   113MB
react-app                        1                50d6cd640827   5 hours ago   113MB
react-app                        2                50d6cd640827   5 hours ago   113MB
react-app                        3                50d6cd640827   5 hours ago   113MB
react-app                        latest           50d6cd640827   5 hours ago   113MB
829282/hello-docker-repository   2                50d6cd640827   5 hours ago   113MB
<none>                           <none>           9feb485cbd35   7 hours ago   113MB
node                             lts-alpine3.16   b48aa2e90c0a   3 days ago    112MB
ubuntu                           latest           27941809078c   4 weeks ago   77.8MB
===================


# this is not work 
docker image rm 50d6cd640827

===================
Error response from daemon: conflict: unable to delete 50d6cd640827 (must be forced) - image is referenced in multiple repositories
===================

# to work you should use -f but this is also delete any image that have this id
docker image rm -f 50d6cd640827

===================
Untagged: 829282/hello-docker-repository:2
Untagged: 829282/hello-docker-repository@sha256:7d1665e1688a6d84cc331d99f4f607b7ffd69bc88f0c55e6d1b1b077aa2b9b25
Untagged: react-app:1
Untagged: react-app:2
Untagged: react-app:3
Untagged: react-app:latest
Deleted: sha256:50d6cd64082723b08ebe96d3cc3981331c99f0b3a18b43b451a1518547ec2346
Deleted: sha256:f4af8d15b0e2756308b843774d3137ad83223a2712b0404b5d4ae6377494b190
Deleted: sha256:173dfad9ff6fb50a34b593e88e0b5880f40f86d70f9eba8e97da85e527844b10
===================

# if you don't won't happen this use the name of the repository also this tag name that have like this
docker image rm react-app:3

# now load the image
docker image load -i react-app.tar

===================
9ed9a9672c53: Loading layer [==================================================>] 869.9kB/869.9kB
Loaded image: react-app:3
===================


# done
docker images

===================
REPOSITORY                       TAG              IMAGE ID       CREATED       SIZE
829282/hello-docker-repository   5                2b579ddfacdc   5 hours ago   113MB
react-app                        4                2b579ddfacdc   5 hours ago   113MB
react-app                        5                2b579ddfacdc   5 hours ago   113MB
react-app                        3                50d6cd640827   6 hours ago   113MB
<none>                           <none>           9feb485cbd35   7 hours ago   113MB
node                             lts-alpine3.16   b48aa2e90c0a   3 days ago    112MB
ubuntu                           latest           27941809078c   4 weeks ago   77.8MB
===================

```

------------------------------

* working with starting container

```bash

# if you are type this you will see not container start to do that follow the step
docker ps

===================
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
===================




# running container in background with custome name
docker run -d --name blue-sky react-app

===================
6c0d00bd823a080b2fbac3c9d003297642e0407c892b742ed07f83008ab6b5da
===================


docker ps

===================
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
===================


docker ps -a

===================
CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS                     PORTS     NAMES
6c0d00bd823a   react-app   "docker-entrypoint.s…"   7 seconds ago   Exited (0) 6 seconds ago             blue-sky
2fa0bb8fdb03   react-app   "docker-entrypoint.s…"   7 minutes ago   Exited (0) 4 minutes ago             angry_dubinsky
===================


# running container in background
docker run -d react-app

===================
2c62ea8d5785529a2392f2469ee3f7faafc73c3eca84b35ee2c2ec62399217b2
===================

docker ps -a

===================
CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS                     PORTS     NAMES
2c62ea8d5785   react-app   "docker-entrypoint.s…"   3 seconds ago   Exited (0) 2 seconds ago             sweet_rosalind
6c0d00bd823a   react-app   "docker-entrypoint.s…"   2 minutes ago   Exited (0) 2 minutes ago             blue-sky
2fa0bb8fdb03   react-app   "docker-entrypoint.s…"   9 minutes ago   Exited (0) 6 minutes ago             angry_dubinsky
===================


```

-------------------------------

* docker log error

```bash

docker ps -a

===================
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS                      PORTS     NAMES
2c62ea8d5785   react-app   "docker-entrypoint.s…"   9 minutes ago    Exited (0) 9 minutes ago              sweet_rosalind
6c0d00bd823a   react-app   "docker-entrypoint.s…"   11 minutes ago   Exited (0) 11 minutes ago             blue-sky
2fa0bb8fdb03   react-app   "docker-entrypoint.s…"   18 minutes ago   Exited (0) 16 minutes ago             angry_dubinsky
===================


docker logs --help

===================

Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
===================


docker logs 2c62ea8d5785

===================
# one time running and check if there is error
===================


docker logs -f 2c62ea8d5785

===================
# real time running this is check in real time if there is error with your app this is usefull if your application produce output in the server or in the docker
===================

docker logs -n 5 2c62ea8d5785

===================
# this is show you the last 5 line this is usefull when your docker output is very very long
===================


docker logs -t 2c62ea8d5785

===================
# show you time stamp for each line that produce in prompt
===================

```

---------------------------

* working with port

```bash

docker ps

===================
CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS                       PORTS     NAMES
a9baebe36ba1   dd5a8213bb21   "docker-entrypoint.s…"   About a minute ago   Exited (130) 2 seconds ago             serene_euclid
===================

# this is means port 3000 in my machine lesson to port 3000 in the container
docker run -d -p 3000:3000 --name c1 react-app

or 

docker run -d -p 80:3000 --name c1 react-app

===================
# note: this is may take some time means take some time for the container to start your project means to show it in ( docker ps )
4d15b4bc626ab245608cb9671471f1c73c3035562d996411428e8f4198009572
===================


# go to browser and type this linke: localhost:3000 if you don't see it then start the container

docker ps -a

===================
docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                            PORTS                                       NAMES
4d15b4bc626a   react-app      "docker-entrypoint.s…"   4 seconds ago   Up 4 seconds                      0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
a9baebe36ba1   dd5a8213bb21   "docker-entrypoint.s…"   2 minutes ago   Exited (130) About a minute ago 
===================


docker ps

===================
CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS         PORTS                                       NAMES
4d15b4bc626a   react-app   "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
===================


```

---------------------------------

* run image in different way

```bash

docker ps

===================
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                                       NAMES
4d15b4bc626a   react-app   "docker-entrypoint.s…"   50 minutes ago   Up 50 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
===================

# this is mean run container that name is c1 and the run should be for the container it self not run it in new container
# after container name you type any command line bash
docker exec -it c1 sh

===================
/app #
===================


docker exec -it c1 ls

===================
Dockerfile         node_modules       package.json       src
README.md          package-lock.json  public             yarn.lock
===================

# create new container and run it
docker run -it c1




docker ps

===================
CONTAINER ID   IMAGE       COMMAND                  CREATED             STATUS             PORTS                                       NAMES
4d15b4bc626a   react-app   "docker-entrypoint.s…"   About an hour ago   Up About an hour   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
===================



docker ps -a

===================
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS                           PORTS                                       NAMES
4d15b4bc626a   react-app      "docker-entrypoint.s…"   About an hour ago   Up About an hour                 0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
a9baebe36ba1   dd5a8213bb21   "docker-entrypoint.s…"   About an hour ago   Exited (130) About an hour ago
===================


```

-------------------------------

* start and stoping the container

```bash

docker ps

===================
docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED             STATUS        PORTS                                       NAMES
4d15b4bc626a   react-app   "docker-entrypoint.s…"   About an hour ago   Up 1 second   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
===================

docker ps -a

===================
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS                           PORTS     NAMES
4d15b4bc626a   react-app      "docker-entrypoint.s…"   About an hour ago   Exited (0) 2 seconds ago                   c1
a9baebe36ba1   dd5a8213bb21   "docker-entrypoint.s…"   About an hour ago   Exited (130) About an hour ago             serene_euclid
===================



docker stop c1

===================
c1
===================


docker ps

===================
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
===================



docker ps -a

===================
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS                           PORTS     NAMES
4d15b4bc626a   react-app      "docker-entrypoint.s…"   About an hour ago   Exited (0) 2 seconds ago                   c1
a9baebe36ba1   dd5a8213bb21   "docker-entrypoint.s…"   About an hour ago   Exited (130) About an hour ago             serene_euclid
===================






docker start c1

===================
c1
===================


docker ps

===================
docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED             STATUS        PORTS                                       NAMES
4d15b4bc626a   react-app   "docker-entrypoint.s…"   About an hour ago   Up 1 second   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
===================


docker ps -a

===================
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS                           PORTS     NAMES
4d15b4bc626a   react-app      "docker-entrypoint.s…"   About an hour ago   Exited (0) 2 seconds ago                   c1
a9baebe36ba1   dd5a8213bb21   "docker-entrypoint.s…"   About an hour ago   Exited (130) About an hour ago             serene_euclid
===================


```

-------------------------------

* removing container

```bash

docker rm musing_cannon

===================
Error response from daemon: You cannot remove a running container 4d15b4bc626ab245608cb9671471f1c73c3035562d996411428e8f4198009572. Stop the container before attempting removal or force remove
===================

docker stop musing_cannon

docker rm musing_cannon

===================
musing_cannon
===================

# or do in that way
docker rm -f musing_cannon

===================
musing_cannon
===================

# see the stoping container
docker ps -a

===================
CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS                           PORTS                                       NAMES
19ee9a1d2e59   react-app      "docker-entrypoint.s…"   About a minute ago   Exited (0) About a minute ago                                                silly_moore
4d15b4bc626a   react-app      "docker-entrypoint.s…"   About an hour ago    Up 12 minutes                    0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
a9baebe36ba1   dd5a8213bb21   "docker-entrypoint.s…"   About an hour ago    Exited (130) About an hour ago                                               serene_euclid
===================

# seeing the running container
docker ps 

===================
CONTAINER ID   IMAGE       COMMAND                  CREATED             STATUS          PORTS                                       NAMES
4d15b4bc626a   react-app   "docker-entrypoint.s…"   About an hour ago   Up 14 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
===================


# looking just for c1
docker ps -a | grep c1

===================
4d15b4bc626a   react-app      "docker-entrypoint.s…"   About an hour ago   Up 15 minutes                    0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
===================

# removing the stoping container
docker container prune

```

----------------------------

* small work

```bash

docker ps

===================
19ee9a1d2e59   react-app   "docker-entrypoint.s…"   17 minutes ago   Up 3 seconds    3000/tcp                                    silly_moore
4d15b4bc626a   react-app   "docker-entrypoint.s…"   2 hours ago      Up 28 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1                                               serene_euclid
===================

# use this two container 
19ee9a1d2e59
4d15b4bc626a



# exec command just work on this container that is running actually
docker exec -it 19ee9a1d2e59 sh

===================
/app # ls
Dockerfile         README.md          node_modules       package-lock.json  package.json       public             src                yarn.lock
/app # echo data > data.txt
/app # exit
===================


docker exec -it 4d15b4bc626a sh

===================
/app # cat data.txt
/app # ls | grep data
===================


```

------------------------

* volume

```bash

docker volume 

===================

Usage:  docker volume COMMAND

Manage volumes

Commands:
  create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes

Run 'docker volume COMMAND --help' for more information on a command.
===================


docker volume create app-data

===================
app-data
===================


docker volume inspect app-data

===================
[
    {
        "CreatedAt": "2022-07-12T11:35:29+03:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/app-data/_data",
        "Name": "app-data",
        "Options": {},
        "Scope": "local"
    }
]
===================




docker ps -a

===================
CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS                      PORTS     NAMES
19ee9a1d2e59   react-app      "docker-entrypoint.s…"   9 hours ago    Exited (137) 7 hours ago              silly_moore
4d15b4bc626a   react-app      "docker-entrypoint.s…"   10 hours ago   Exited (0) 7 hours ago                c1
a9baebe36ba1   dd5a8213bb21   "docker-entrypoint.s…"   11 hours ago   Exited (130) 10 hours ago             serene_euclid
===================


docker ps 

===================
CONTAINER ID   IMAGE       COMMAND                  CREATED        STATUS          PORTS                                       NAMES
4d15b4bc626a   react-app   "docker-entrypoint.s…"   10 hours ago   Up 10 seconds   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
===================



# -d    ( detach mod )
# -p    ( port )
# -v    ( volume )
# if name ( app-data ) is not exist then the docker create one for you after volume name you should put the path in my case for react-app is /app/data
docker run -d -p 4000:3000 -v app-data:/app/data react-app

===================
# this unique id is generated by docker it will create the image but it may be take some time
b35b8e5295ed6422c5499f553bdc60974ff35f2ace95590a6f284c572d65c383
===================


docker ps

===================
CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS              PORTS                                       NAMES
# look at first image it match this ( b35b8e5295ed6422c5499f553bdc60974ff35f2ace95590a6f284c572d65c383 ) and you should start it
b35b8e5295ed   react-app   "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes        0.0.0.0:4000->3000/tcp, :::4000->3000/tcp   musing_carver
19ee9a1d2e59   react-app   "docker-entrypoint.s…"   9 hours ago     Up About a minute   3000/tcp                                    silly_moore
4d15b4bc626a   react-app   "docker-entrypoint.s…"   11 hours ago    Up 4 minutes        0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   c1
===================


docker exec -it b35b8e5295ed sh

===================
/app # ls
Dockerfile         README.md          data               node_modules       package-lock.json  package.json       public             src                yarn.lock
/app # cd data
/app/data # ls
/app/data # echo hello > helloworld.txt
/app/data # ls
helloworld.txt
/app/data # exit
===================


# now if you delete this container this folder will be still exist when you create other container with same port

docker rm -f b35b8e5295ed

===================
b35b8e5295ed
===================


docker run -d -p 4000:3000 -v app-data:/app/data react-app

===================
60a35871531944ee725d915f17720e3a68cc8ea9ba324951b636b73ffeb48af6
===================


docker exec -it 60a sh

===================
/app # cd data
/app/data # ls
helloworld.txt
/app/data # cat helloworld.txt
hello
/app/data # 
===================

```

* there is a problem here if you connect with this docker as normal user not like root user you can't do anything in that folder because the permission this folder will be this ( drwxr-xr-x ) means the user it can't write in that folder to fix that you can create folder when you build the project

```dockerfile

FROM node:14.16.0-alpine3.13
RUN addgroup app && adduser -S -G app app
USER app
WORKDIR /app
RUN mkdir data
COPY package*.json .
COPY . . 
RUN npm install
ENV API_URL=http://api.myapp.com/
EXPOSE 3000
CMD ["npm", "start"]

```

```bash

docker build -t react-app .

docker run -d -p 5000:3000 -v app-data:/app/data react-app

===================
cbd7a7479440ae1862389d29db097712af1ff8feed3d36649a1beb43ad7fd00a
===================


docker exec -it cbd sh

===================
/app $ ls
Dockerfile         README.md          data               node_modules       package-lock.json  package.json       public             src                yarn.lock
/app $ cd Data
sh: cd: can't cd to Data: No such file or directory
/app $ cd data
/app/data $ ls
hello           helloworld.txt
/app/data $ cat helloworld.txt
hello
/app/data $ 
===================

```