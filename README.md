# Fundamental_of_Docker
Fundamentals of Docker, installation, commands, images, containers, volumes, pushing to Docker hub, build with Docker, integrate with other tools, debug and troubleshooting.

What is a container?

Simply put, a container is a sandboxed process on your machine that is isolated from all other processes on the host machine. That isolation leverages kernel namespaces and cgroups, features that have been in Linux for a long time. Docker has worked to make these capabilities approachable and easy to use. To summarize, a container:

Is a runnable instance of an image. You can create, start, stop, move, or delete a container using the DockerAPI or CLI.

Can be run on local machines, virtual machines or deployed to the cloud.

Is portable (can be run on any OS).

Is isolated from other containers and runs its own software, binaries, and configurations.

What is a container image?

When running a container, it uses an isolated filesystem. This custom filesystem is provided by a container image. Since the image contains the container’s filesystem, it must contain everything needed to run an application - all dependencies, configurations, scripts, binaries, etc. The image also contains other configuration for the container, such as environment variables, a default command to run, and other metadata.


Installing Docker on Ubuntu 
 		 

uninstalling any old versions

sudo apt remove docker docker.io containerd runc
 		 
 		 
adding Docker’s official GPG signing key:

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 		 
 		 
adding the official docker repository

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
 		 
refreshing the apt cache

sudo apt update
 		 
selecting the docker repository as the default one

apt-cache policy docker-ce
 		 
installing docker

sudo apt install docker-ce docker-ce-cli containerd.io
 		 
checking its status

sudo systemctl status docker

<img width="859" alt="Screenshot 2023-05-25 at 00 22 00" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/8bbb5bd6-ac81-44cf-b774-c5b02587090e">
 		 
adding the current user to the docker group to be able to run the docker command

sudo usermod -aG docker ${USER}

<img width="844" alt="Screenshot 2023-05-25 at 00 23 39" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/a8623651-5994-465f-8e99-72ce55772086">


 		
 		 
testing the entire installing

docker --version

docker version

docker info

docker container run hello-world

<img width="866" alt="Screenshot 2023-05-25 at 00 25 14" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/8c93a6fb-58b6-4752-b082-d0e099e7b4f6">

<img width="860" alt="Screenshot 2023-05-25 at 00 26 04" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/46f7e872-1424-4748-8fd8-59f81bcfe1aa">
 		 
getting help

docker help

docker MANAGEMENT_COMMAND help => Ex: docker container help
 		 
searching for an image on Docker Hub

docker search IMAGE_NAME

docker search debian

docker search mongo
 		 
pulling an image from docker hub

docker image pull IMAGE_NAME:TAG
docker image pull redis:5.0.10
docker image pull ubuntu:latest
docker image pull mysql # => by default tha name of the TAG is latest
 		 
listing local images

docker images

docker image ls
		 
running a container

docker container run OPTIONS IMAGE_NAME

docker container run -P httpd

equivalent to:

docker container create -P httpd

docker container ls -a # listing all containers

docker container start CONTAINER_ID
		 
getting a shell into a container

docker container run -it centos # to detach from the container without stopping it press: Ctrl + P + Q

Running a Web Server in a Docker Container ##

docker container run -d --p 80:80 --name mysite1 nginx

docker container run -d --p 8080:80 --name mysite2 nginx

docker container run -d --p 8081:80 --name mysite3 nginx
		 
-d => detach and run in the background

--name => container name, must be unique, randomly chosen from a list if it's not given

-p X:Y => publish a container's port to the host

process listens on port Y in the container but is accessed on port X from the outside (LAN/Internet)

-P => publish all exposed ports to random ports
 		 
listing local images

docker images # old command

docker image ls # new command
 		 
listing all running containers

docker ps # old command

docker container ls # new command

-q => printing only the containers' ids
 		 

listing all containers (created, running, stopped)

docker ps -a

docker container ls -a
 		 
filtering by status

docker container ls -a -f status=exited
 		 

stopping a container

docker container stop CONTAINER_ID|CONTAINER_NAME

Example: docker container stop mysite1
 		 
removing a container (must be stopped)

docker container rm CONTAINER_ID|CONTAINER_NAME

-f => force remove the container (can be running)

Example: docker container rm mysite1
 		 

removing all stopped containers

docker container rm $(docker container ls -f status=exited -q)
 		 
removing an image

docker rmi IMAGE_NAME # => old command

docker image rm IMAGE_NAME # => new command

Example: docker image rm nginx
		 
removing dangling images, stopped containers, dangling build cache and networks not used

docker system prune

-a => remove unused images, as well

getting shell access to the container

docker run -it --name=container1 centos => press Ctrl+P +Q to exit the container without stopping it
 		 
getting a shell in a running container

docker container exec -it CONTAINER_ID|CONTAINER_NAME bash
Example: docker container exec -it container1 bash
 		 
executing shell commands in running container

docker container exec CONTAINER_ID|CONTAINER_NAME COMMAND

Examples: docker container exec container1 cat /etc/shadow

docker container exec container1 yum -y install nmap
 		 
stopping a running container

docker container stop CONTAINER_ID|CONTAINER_NAME
 		 
starting a stopped container

docker container start CONTAINER_ID|CONTAINER_NAME
 		 
removing a stopped container

docker container rm CONTAINER_ID|CONTAINER_NAME
		 
removing a running container

docker container rm -f CONTAINER_ID|CONTAINER_NAME

Commands - Committing Changes, Tagging and Pushing Images

Commiting changes in a container to a new image
STEP 1

start the container, get shell access, make any changes and exit

docker container run -it --name=container1 centos

STEP 2

create a new image from the modified container

docker commit -m "What did you do to the image" -a "Author Name" CONTAINER_ID|CONTAINER_NAME repository/new_image_name:tag

Example: docker commit -m "nmap installed" -a "Andrei D." container1 ddandrei/my_centos

STEP 3

start containers from the image

docker image ls

docker container run -it ddandrei/my_centos

adding a new tag to an existing image

docker image tag nginx ddandrei/nginx:custom

docker image tag ddandrei/my_centos:latest ddandrei/my_centos:1.0

docker image ls

pushing custom images to docker Hub

STEP 1

create an account on hub.docker.com

STEP 2

docker login  => enter username and password

STEP 3

docker image push ddandrei/nginx:custom  # => the username on docker hub must match the image's repository (ddandrei)

Commands - Volumes

creating a new volume

docker volume create mysite
 		 
listing all volumes

docker volume ls
 		 
inspecting a volume

docker volume inspect mysite
 		 
removing a volume

docker volume rm mysite

docker volume prune     # => remove all unused volumes
 		 
starting a container with the volume

docker container run -d --name mywebapp -p 80:80 -v mysite:/usr/share/nginx/html nginx
























