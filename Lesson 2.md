## simple code to run with docker

```js

# create file name is app.js and type this inside it
console.log("hello world");



# to run this in your machine
node app.js

```

* If you want run your program in other computer you should do this or install it in virtual machine

    - Starting with an OS
    
    - Installing Node
    
    - Copy app files
    
    - Run node app.s

* to know if my package or software or tool or system is available for docker go to docker website

    - Link: https://hub.docker.com/

    - to know if node is availabe search for it in my case is exist:

        + https://hub.docker.com/_/node

* now we need docker file

    - create file name is: Dockerfile

```yml

# means use alpine container and run node in top of the alpine
# now the node application run in alpine linux container
FROM node:alpine
# meas copy all file in app directory folder
COPY . /app
# any command or depedence i use is in app directory
WORKDIR /app
# run app.js use node
CMD node app.js





# after that run this command
sudo docker build -t hello-docker .

===
Sending build context to Docker daemon  3.072kB
Step 1/4 : FROM node:alpine
alpine: Pulling from library/node
2408cc74d12b: Pull complete 
8283096dcba0: Pull complete 
a3e654983208: Pull complete 
024c9a3b77d6: Pull complete 
Digest: sha256:7ae41699c38d8e50f5bf592867cf661368d71ff922e07f6f66f36dca2ff0c590
Status: Downloaded newer image for node:alpine
 ---> 515db77e67c7
Step 2/4 : COPY . /app
 ---> 5a2243e6fbec
Step 3/4 : WORKDIR /app
 ---> Running in 54d49e12188b
Removing intermediate container 54d49e12188b
 ---> 2bcf1221089b
Step 4/4 : CMD node app.js
 ---> Running in a09e4fdcc8af
Removing intermediate container a09e4fdcc8af
 ---> 55422aa0d921
Successfully built 55422aa0d921
Successfully tagged hello-docker:latest
===



# see if my image is exist
sudo docker image ls                       or                docker images

===
REPOSITORY     TAG       IMAGE ID       CREATED              SIZE
hello-docker   latest    55422aa0d921   About a minute ago   174MB
node           alpine    515db77e67c7   2 weeks ago          174MB
===




# run your small project
docker run hello-docker


# if you want run this image in other computer for now i use play with docker website link: https://labs.play-with-docker.com/
# if you want run your small project in anywhere then import it to docker website and pull it in this computer that you want
docker pull hello-docker

===
# this will be install all depedence
.
.
.
.
===

docker run hello-docker

```


