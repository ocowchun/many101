docker 
packer
use docker and packer to package rails app


##todo 
practice docker


###docker machine
docker-machine讓你可以在你的vm上執行docker，這是一個簡化Docker安裝的工具，透過簡單的命令，就可以在相對應的平台上安裝docker。


###before-start
開始執行docker的command前，先將docker-machine的相關設定整合進你的command line
可以透過執行`docker-machine env <machine-name>`看到相關的設定。


run 


```bash
#create docker machine
$ docker-machine create --driver virtualbox dev

#stop docker machine
$ docker-machine stop default

# Lists containers.
$ docker ps

#Shows us the standard output of a container.
$ docker logs container-name

#Stops running containers.
$ docker stop container-name

#remove container
$ docker rm container-name

#return container mapped port
$ docker port container-name

#causes the docker logs command to act like the tail -f command and watch the container’s standard out. 
$ docker logs -f container-name

#examine the processes running container
$ docker top container-name

# pre-load an image
$ docker pull image-name

#remove images on your Docker
$ docker rmi image-name

```

###docker
```bash
$ docker run -t -i ubuntu:14.04 /bin/bash
root@af8bae53bdd3:/#
```

###common flags

The `-t` flag assigns a pseudo-tty or terminal inside our new container
The `-i` flag allows us to make an interactive connection by grabbing the standard in (STDIN) of the container.
The `-d` flag tells Docker to run the container and put it in the background, to daemonize it
The `-P` tells Docker to map any required network ports inside our container to our host. This lets us view our web application


###Creating our own images
There are two ways you can update and create images.

* You can update a container created from an image and commit the results to an image.
* You can use a Dockerfile to specify instructions to create an image.

####Updating and committing an images

```bash
$ docker commit -m "your-commit-message" -a "author-name" container-id ouruser/sinatra:v2
```


```bash
$ docker build -t ouruser/sinatra:v2 .
```
You’ve specified our docker build command and used the -t flag to identify our new image as belonging to the user ouruser, the repository name sinatra and given it the tag v2.


#todo
https://docs.docker.com/engine/userguide/dockervolumes/
