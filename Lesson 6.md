* optimizing the speed of building

```bash

# each line in docker ( from run workdir copy add env expose ... ) is a layer and each of this layer can have size for building for example 

docker history react-app

===
# you can see in column ( CREATED BY ) that what script of docker is running and in ( SIZE ) column you can see the size of this script docker take for excute 
# find this command that take more and optimize it in this case is my ( COPY ) how to optimize it
IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
7662953459c2   29 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "npm …   0B        
7d4249875a2a   29 minutes ago   /bin/sh -c #(nop)  EXPOSE 3000                  0B        
6dd259a3305c   29 minutes ago   /bin/sh -c #(nop)  ENV API_URL=http://api.my…   0B        
b264a688fee9   29 minutes ago   /bin/sh -c #(nop) COPY dir:f1b8367d9b386d9cf…   227MB     
e3d06b77ce04   2 hours ago      /bin/sh -c #(nop) WORKDIR /app                  0B        
b48aa2e90c0a   2 days ago       /bin/sh -c #(nop)  CMD ["node"]                 0B        
<missing>      2 days ago       /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B        
<missing>      2 days ago       /bin/sh -c #(nop) COPY file:4d192565a7220e13…   388B      
<missing>      2 days ago       /bin/sh -c apk add --no-cache --virtual .bui…   7.76MB    
<missing>      2 days ago       /bin/sh -c #(nop)  ENV YARN_VERSION=1.22.19     0B        
<missing>      2 days ago       /bin/sh -c addgroup -g 1000 node     && addu…   98.7MB    
<missing>      2 days ago       /bin/sh -c #(nop)  ENV NODE_VERSION=16.16.0     0B        
<missing>      6 weeks ago      /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B        
<missing>      6 weeks ago      /bin/sh -c #(nop) ADD file:8e81116368669ed3d…   5.53MB  
===

```


```dockerfile

FROM node:lts-alpine3.16
RUN addgroup app && adduser -S -G app alex
USER alex
WORKDIR /app
# now you can see that we just need install the package and we don't need more that this this is only thing that take more time this will be first time take along time but after for second and third and ... it just take one second
COPY package*.json .
RUN npm install
COPY . .
ENV API_URL=http://api.myapp.com/
EXPOSE 3000
CMD npm start
# CMD ["npm", "start"]

```

* run and build

```bash

docker build -t react-app .

===
# first time take a lot of time
Sending build context to Docker daemon  259.4MB
Step 1/8 : FROM node:lts-alpine3.16
 ---> b48aa2e90c0a
Step 2/8 : WORKDIR /app
 ---> Using cache
 ---> e3d06b77ce04
Step 3/8 : COPY package*.json .
 ---> ee5b545c0645
Step 4/8 : RUN npm i npm
 ---> 80a3bda8620a
Step 5/8 : COPY . .
 ---> Using cache
 ---> a22e075fbfc6
Step 6/8 : ENV API_URL=http://api.myapp.com/
 ---> Using cache
 ---> e9e5ac8d647d
Step 7/8 : EXPOSE 3000
 ---> Using cache
 ---> 578c7b7342f0
Step 8/8 : CMD npm start
 ---> Using cache
 ---> 14c50b59ea20
Successfully built 14c50b59ea20
Successfully tagged react-app:latest
===

docker build -t react-app .

===
# second time if you look at it uses cache this time
Sending build context to Docker daemon  259.4MB
Step 1/8 : FROM node:lts-alpine3.16
 ---> b48aa2e90c0a
Step 2/8 : WORKDIR /app
 ---> Using cache
 ---> e3d06b77ce04
Step 3/8 : COPY package*.json .
 ---> Using cache
 ---> ee5b545c0645
Step 4/8 : RUN npm i npm
 ---> Using cache
 ---> 80a3bda8620a
Step 5/8 : COPY . .
 ---> Using cache
 ---> a22e075fbfc6
Step 6/8 : ENV API_URL=http://api.myapp.com/
 ---> Using cache
 ---> e9e5ac8d647d
Step 7/8 : EXPOSE 3000
 ---> Using cache
 ---> 578c7b7342f0
Step 8/8 : CMD npm start
 ---> Using cache
 ---> 14c50b59ea20
Successfully built 14c50b59ea20
Successfully tagged react-app:latest
===

```

-------------------------------

* removing useless images and container

```bash

# see if there is any running container
docker ps 

===
CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS                           PORTS     NAMES
===

# see the stoping container
docker ps -a

===

CONTAINER ID   IMAGE          COMMAND                  CREATED             STATUS                           PORTS     NAMES
2c712d65ecd7   ee5b545c0645   "/bin/sh -c 'npm i n…"   4 minutes ago       Up 4 minutes                               stupefied_borg
ffab7b5da475   ee5b545c0645   "/bin/sh -c 'npm i n…"   18 minutes ago      Exited (1) 8 minutes ago                   brave_elgamal
37956457398a   d9acc59eb860   "docker-entrypoint.s…"   About an hour ago   Exited (1) About an hour ago               goofy_visvesvaraya
720bd0e6c7e4   4ccb0605350e   "docker-entrypoint.s…"   About an hour ago   Exited (1) About an hour ago               cranky_wilson
379df368d584   4ccb0605350e   "docker-entrypoint.s…"   About an hour ago   Exited (1) About an hour ago               lucid_davinci
fce9439ea11a   4ccb0605350e   "docker-entrypoint.s…"   About an hour ago   Exited (1) About an hour ago               laughing_almeida
06b29fe824f6   4ccb0605350e   "docker-entrypoint.s…"   About an hour ago   Exited (1) About an hour ago               charming_borg
cdd9a81c5789   d080a7fd70a6   "docker-entrypoint.s…"   2 hours ago         Exited (130) About an hour ago             focused_germain

===

docker container prune

===
# this will remove any useless container
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
ffab7b5da475330a66d2dc5be50645c93e7d4d909ab48d8239de53bad4bc4ba2
37956457398a108551e102a1871f0ccfadef30b2f1928774a88eda8faa03300a
720bd0e6c7e4565b9aa67f77dbd7093a1448bc5d37c95c8428fd11d71048d194
379df368d5843c10c77be38989a847f788690dce7a5c9819b7239d194b169c22
fce9439ea11a40d04bc61b4f923264baa7f5b3837f35b92824d9eaf829cbcbba
06b29fe824f6036ede7dd07988939197c827a4970eb0e2c128bd47cc9b6e552f
cdd9a81c57895492173c051defddda6fd536dc8b0aabf1836568d5d7053dc5e7

Total reclaimed space: 506.3MB

===


# look for image for this contianer and remove it
docker images

===
# this image that not have repository and tag this image is useless means it is run multiple time for small changes you can remove all in this this way in below
REPOSITORY   TAG              IMAGE ID       CREATED             SIZE
<none>       <none>           ee5b545c0645   16 minutes ago      112MB
react-app    latest           7662953459c2   52 minutes ago      339MB
<none>       <none>           d9acc59eb860   About an hour ago   339MB
<none>       <none>           4ccb0605350e   About an hour ago   339MB
<none>       <none>           d080a7fd70a6   2 hours ago         113MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago          112MB
===


docker image prune

===

WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N] y
Deleted Images:
deleted: sha256:4ccb0605350e1c4c9b53d381a6ce92983cd1121fe201215249834535748d232d
deleted: sha256:99fbb0ea3e7e9abd685619cf3fe8fa57427c12cb89ae7986bb0ef51575dc1e0c
deleted: sha256:d72f4e7de285642ae804a6495f056fc6eeb88e0168cc0d075fc92cfcfe21a208
deleted: sha256:ec97e89d9796f6e9b61a5a5013c463149d68a27c198d6010a91dde8f75ed9417
deleted: sha256:22815addc0bc4d05cbe59fcd4c9df60804df1d1f8b35bb4217e2d83e858a0efc
deleted: sha256:1605a788ee2e88897e4dda5be6ec4039288422206eef1dd20a90094ef53abb10
deleted: sha256:548571ad6990c1abc24a23ae8b4de4a0a05f02061689703f5ba482b1ba172897
deleted: sha256:570cda1f201664675db21335445951d8014e920d55e8ed34dc77fe3040295ba6
deleted: sha256:8599e443ba45b5e04f63720a868aeeff19950de421398d6ccb6f0d32faaea5fa
deleted: sha256:4adc9efb70a506c889f90044d7ea651ce4fdd4a9835ecd61548949467186ee9c
deleted: sha256:d9acc59eb860c0331be7ceb24fef79e907ad0d8ad24e7826459eb8bbce1224bb
deleted: sha256:5e5c369cd6ef8406999c31dfa4c722b6606b91a3a57db058e72a11e218afac79
deleted: sha256:6592ee8f8a5a87696ee962823475a65f5a27384540f4052873a4967084076966
deleted: sha256:07121b495f1589df2d6f754b0041354339ee9cb6b1bb2fb964130b0a7721a29e
deleted: sha256:b9d95f49f13542ab156eb4a518c287c33e75b977b9d8aac32e16549fce3983f1
deleted: sha256:d080a7fd70a6b0d1c97d8aeba70f82fc437e6e63180256b7875240abfc52ef3d
deleted: sha256:b9f3a83869ce827c4eed9481bf8b33a717a4320ea06d6e89345fb530e77c94d1
deleted: sha256:ebea7749367985878bdb25a399d9b72cf943d898beca63fb9e95009752f0b69d
deleted: sha256:e6a16342ecf3c6f4c98f0631667398363200b566469c47914acaccc5bf011126

Total reclaimed space: 454.5MB
===


docker images

===
REPOSITORY   TAG              IMAGE ID       CREATED          SIZE
<none>       <none>           80a3bda8620a   6 seconds ago    495MB
react-app    latest           7662953459c2   57 minutes ago   339MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago       112MB
[15:14:16] soyansoon@soyansoon-Blade  $      ~/Documents/Study/M
===

docker ps -a

===
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
===



# mroe info about docker image
docker image

===
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
===

```

---------------------------------

*  learn about tag and name the tag

```bash

# see the tag
docker images

===
# by default the Tag is lates this is not means that is latest version of this program or os you are using means it is latest version of this modifyer that you are modify it
REPOSITORY   TAG              IMAGE ID       CREATED         SIZE
react-app    latest           50d6cd640827   2 minutes ago   113MB
<none>       <none>           9feb485cbd35   2 hours ago     113MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago      112MB
ubuntu       latest           27941809078c   4 weeks ago     77.8MB
===

docker build -t react-app .

docker images

===
# it is steal the latest tag for this new build that we made
REPOSITORY   TAG              IMAGE ID       CREATED         SIZE
react-app    latest           50d6cd640827   4 minutes ago   113MB
<none>       <none>           9feb485cbd35   2 hours ago     113MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago      112MB
ubuntu       latest           27941809078c   4 weeks ago     77.8MB
===



# leat's name the tag
docker build -t react-app:1 .

===
# you can see there is new that have the same image id for previous react-app
REPOSITORY   TAG              IMAGE ID       CREATED         SIZE
react-app    1                50d6cd640827   5 minutes ago   113MB
react-app    latest           50d6cd640827   5 minutes ago   113MB
<none>       <none>           9feb485cbd35   2 hours ago     113MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago      112MB
ubuntu       latest           27941809078c   4 weeks ago     77.8MB
===



# how to remove it when we have the same image id like this we do
docker image remove react-app:1

===
Untagged: react-app:1
===



docker images

===
REPOSITORY   TAG              IMAGE ID       CREATED         SIZE
react-app    latest           50d6cd640827   7 minutes ago   113MB
<none>       <none>           9feb485cbd35   2 hours ago     113MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago      112MB
ubuntu       latest           27941809078c   4 weeks ago     77.8MB
===


# rename the tag
docker image tag react-app:latest react-app:1



docker images

===
REPOSITORY   TAG              IMAGE ID       CREATED          SIZE
react-app    1                50d6cd640827   10 minutes ago   113MB
react-app    latest           50d6cd640827   10 minutes ago   113MB
<none>       <none>           9feb485cbd35   2 hours ago      113MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago       112MB
ubuntu       latest           27941809078c   4 weeks ago      77.8MB
===




# if you are made some small change into this file that is in your project and build it with different tag or not like this you may see that the image id will be change.
# demo


# if you don't change anything in any file
docker build -t react-app:2 .

docker images

===
# the tag 1 and  2 has the same image id
REPOSITORY   TAG              IMAGE ID       CREATED          SIZE
react-app    1                50d6cd640827   12 minutes ago   113MB
react-app    2                50d6cd640827   12 minutes ago   113MB
react-app    latest           50d6cd640827   12 minutes ago   113MB
<none>       <none>           9feb485cbd35   2 hours ago      113MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago       112MB
ubuntu       latest           27941809078c   4 weeks ago      77.8MB
===


# same other test if i don't change any thing run it again with different tag name
docker build -t react-app:3 .

===
REPOSITORY   TAG              IMAGE ID       CREATED          SIZE
react-app    1                50d6cd640827   12 minutes ago   113MB
react-app    2                50d6cd640827   12 minutes ago   113MB
react-app    3                50d6cd640827   12 minutes ago   113MB
react-app    latest           50d6cd640827   12 minutes ago   113MB
<none>       <none>           9feb485cbd35   2 hours ago      113MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago       112MB
ubuntu       latest           27941809078c   4 weeks ago      77.8MB
===



# if you made the change in the file the image id is change

docker build -t react-app:4 .

docker images

===
REPOSITORY   TAG              IMAGE ID       CREATED          SIZE
react-app    4                2b579ddfacdc   4 seconds ago    113MB
react-app    1                50d6cd640827   19 minutes ago   113MB
react-app    2                50d6cd640827   19 minutes ago   113MB
react-app    3                50d6cd640827   19 minutes ago   113MB
react-app    latest           50d6cd640827   19 minutes ago   113MB
<none>       <none>           9feb485cbd35   2 hours ago      113MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago       112MB
ubuntu       latest           27941809078c   4 weeks ago      77.8MB
===

# this will point tag latest there id to id new image tag 2 
docker image tag 2b579ddfacdc react-app:latest

docker images

===
REPOSITORY   TAG              IMAGE ID       CREATED          SIZE
react-app    4                2b579ddfacdc   4 seconds ago    113MB
react-app    1                50d6cd640827   19 minutes ago   113MB
react-app    2                50d6cd640827   19 minutes ago   113MB
react-app    3                50d6cd640827   19 minutes ago   113MB
react-app    latest           2b579ddfacdc   19 minutes ago   113MB
<none>       <none>           9feb485cbd35   2 hours ago      113MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago       112MB
ubuntu       latest           27941809078c   4 weeks ago      77.8MB
===

```


------------------------------------------

* Push my project to docker to share it with my friend

```bash

# go to docker website and create account
# link: https://www.docker.com/

# after that create repository

# after that  push your project like that:
# name your push should be name your repository like this: 829282/hello-docker-repository

docker images

===
REPOSITORY   TAG              IMAGE ID       CREATED          SIZE
react-app    4                2b579ddfacdc   21 minutes ago   113MB
react-app    1                50d6cd640827   40 minutes ago   113MB
react-app    2                50d6cd640827   40 minutes ago   113MB
react-app    3                50d6cd640827   40 minutes ago   113MB
react-app    latest           50d6cd640827   40 minutes ago   113MB
<none>       <none>           9feb485cbd35   2 hours ago      113MB
node         lts-alpine3.16   b48aa2e90c0a   2 days ago       112MB
ubuntu       latest           27941809078c   4 weeks ago      77.8MB
===




# rename the repository for one of the image
docker image tag 50d6cd640827 829282/hello-docker-repository:2

docker images

===
REPOSITORY                       TAG              IMAGE ID       CREATED          SIZE
react-app                        4                2b579ddfacdc   22 minutes ago   113MB
# now name of the  REPOSITORY of the tag 2 is   829282/hello-docker-repository
829282/hello-docker-repository   2                50d6cd640827   41 minutes ago   113MB
react-app                        1                50d6cd640827   41 minutes ago   113MB
react-app                        2                50d6cd640827   41 minutes ago   113MB
react-app                        3                50d6cd640827   41 minutes ago   113MB
react-app                        latest           50d6cd640827   41 minutes ago   113MB
<none>                           <none>           9feb485cbd35   2 hours ago      113MB
node                             lts-alpine3.16   b48aa2e90c0a   2 days ago       112MB
ubuntu                           latest           27941809078c   4 weeks ago      77.8MB
===



# now login with docker in terminal
docker login

===
Login with your Docker ID to push and pull images from Docker Hub. If you dont have a Docker ID, head over to https://hub.docker.com to create one.
Username: 
Password:
Login Succeded
===


# now push the repository to the internet
# this will push the tag 2 image to the internet
# after that go to internet you will be see the change link: https://hub.docker.com/repository/docker/829282/hello-docker-repository
docker push 829282/hello-docker-repository:2

===
The push refers to repository [docker.io/829282/hello-docker-repository]
9ed9a9672c53: Pushed 
74251841d3a2: Pushed 
78ab8aca0647: Mounted from library/node 
d56a7115c765: Mounted from library/node 
4f9820ca95dd: Mounted from library/node 
24302eb7d908: Mounted from library/node 
2: digest: sha256:7d1665e1688a6d84cc331d99f4f607b7ffd69bc88f0c55e6d1b1b077aa2b9b25 size: 1575
===




# now what happen if i wnat change something in my project like this:
# change something like edit something in your project
# and build it
docker build -t react-app:5 .

===
Sending build context to Docker daemon  867.8kB
Step 1/4 : FROM node:lts-alpine3.16
 ---> b48aa2e90c0a
Step 2/4 : WORKDIR /app
 ---> Using cache
 ---> 8a7f87b897e2
Step 3/4 : COPY . .
 ---> Using cache
 ---> 48dd7d7812b0
Step 4/4 : ENV API_URL=http://api.myapp.com/
 ---> Using cache
 ---> 2b579ddfacdc
Successfully built 2b579ddfacdc
Successfully tagged react-app:5
===


docker images

===
REPOSITORY                       TAG              IMAGE ID       CREATED             SIZE
react-app                        4                2b579ddfacdc   47 minutes ago      113MB
react-app                        5                2b579ddfacdc   47 minutes ago      113MB
react-app                        1                50d6cd640827   About an hour ago   113MB
react-app                        2                50d6cd640827   About an hour ago   113MB
react-app                        3                50d6cd640827   About an hour ago   113MB
react-app                        latest           50d6cd640827   About an hour ago   113MB
829282/hello-docker-repository   2                50d6cd640827   About an hour ago   113MB
<none>                           <none>           9feb485cbd35   3 hours ago         113MB
node                             lts-alpine3.16   b48aa2e90c0a   2 days ago          112MB
ubuntu                           latest           27941809078c   4 weeks ago         77.8MB
===


# rename new tag
docker image tag react-app:5 829282/hello-docker-repository:5

docker images

===
REPOSITORY                       TAG              IMAGE ID       CREATED             SIZE
react-app                        4                2b579ddfacdc   47 minutes ago      113MB
react-app                        5                2b579ddfacdc   47 minutes ago      113MB
829282/hello-docker-repository   5                2b579ddfacdc   47 minutes ago      113MB
829282/hello-docker-repository   2                50d6cd640827   About an hour ago   113MB
react-app                        1                50d6cd640827   About an hour ago   113MB
react-app                        2                50d6cd640827   About an hour ago   113MB
react-app                        3                50d6cd640827   About an hour ago   113MB
react-app                        latest           50d6cd640827   About an hour ago   113MB
<none>                           <none>           9feb485cbd35   3 hours ago         113MB
node                             lts-alpine3.16   b48aa2e90c0a   2 days ago          112MB
ubuntu                           latest           27941809078c   4 weeks ago         77.8MB
===


# and push it
# this time if change is small the push is very fast
docker push 829282/hello-docker-repository:5

===
The push refers to repository [docker.io/829282/hello-docker-repository]
0ae5f1b326e5: Pushed 
74251841d3a2: Layer already exists 
78ab8aca0647: Layer already exists 
d56a7115c765: Layer already exists 
4f9820ca95dd: Layer already exists 
24302eb7d908: Layer already exists 
5: digest: sha256:a1f6e9d0b0fb32faa8cebc8b8c5eb68d178957302054d6b1595d714fa58a7381 size: 1575
===


# if you look at the internet in your repository you will see that new version in tag tab is made link: https://hub.docker.com/repository/docker/829282/hello-docker-repository


```