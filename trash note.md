docker start -i cb878c97e089

docker run -d -p 8000:3000 -v $PWD:/app -it hello-world /bin/sh

docker run -d -v $PWD:/app -it hello-world
