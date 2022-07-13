## What is docker: docker is platform that provide holding your project with this dependence without installing in new machine or setting up the configuration for it.

* Problem Before : 

    - you should have the same version of the ( tool or package or library or ..) that need install it in the machine + this configuration in the setting that need to manage before running the application.

    - More ( Hardware or Software )  in your computer cause performance or diffecalty to run your system and it should be start your application every time before running your project ( slow expernis ) come here for the user.

    - you may need to run other project that have dependent on other ( version of other tool or package ) and also you may be have problem that your other project with different dependent and version ( of this software or tool or package or library ) but this new project or this project in github have different version of previous ( package or library or..) this is problem because your computer have different version of this library or software that you need to done this work.

* Solution : 

    - docker come here to manage all this by:

    - less hardware depende it can run multiple more than 100 handred container and image in the same time without a lot of performance.

    - each container or image run in difference way and different tool and library and .. without interact with each other to make problem to each other. 
    
    - the version you need for each library or … will install for specific image or container this is the best.

* Some Of Note:

??????


* Artichture Of Docker:

    - Docker use rest api to tali with server
    
    - Client in this picture is Docker Engine

??????

## Installing Docker

    link: https://docs.docker.com/engine/install/

    installation for linux: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04

* Some usefull command

docker version

```java

# this is client version

Client: Docker Engine - Community
 Version:           20.10.17
 API version:       1.41
 Go version:        go1.17.11
 Git commit:        100c701
 Built:             Mon Jun  6 23:02:57 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

# this is server version
Server: Docker Engine - Community
 Engine:
  Version:          20.10.17
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.17.11
  Git commit:       a89b842
  Built:            Mon Jun  6 23:01:03 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.6
  GitCommit:        10c12954828e7c7c9b6e0ea9b0c02b01407d3ae1
 runc:
  Version:          1.1.2
  GitCommit:        v1.1.2-0-ga916309
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

```


------------------------------
# installing ubuntu using docker 



```bash

docker pull ubuntu                 or                 docker run ubuntu

===
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
405f018f9d1d: Downloading [============================================>      ]  26.77MB/30.42MB
405f018f9d1d: Pull complete 
Digest: sha256:b6b83d3c331794420340093eb706a6f152d9c1fa51b262d9bf34594887c2c7ac
Status: Downloaded newer image for ubuntu:latest
===





docker run -it ubuntu
 
```