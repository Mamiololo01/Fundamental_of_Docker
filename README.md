# Fundamentals_of_Docker
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
 		 

Uninstalling any old versions

sudo apt remove docker docker.io containerd runc

<img width="851" alt="Screenshot 2023-05-25 at 00 16 02" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/f07747f6-0501-4589-8d79-65b7afdf710f">
 		 
 		 

Adding Docker’s official GPG signing key:

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 		 
 		 
Adding the official docker repository

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

<img width="858" alt="Screenshot 2023-05-25 at 00 19 24" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/d654ad4a-1779-4cba-85a3-be880da69588">
 		 

Refreshing the apt cache

sudo apt update

<img width="819" alt="Screenshot 2023-05-25 at 00 20 01" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/752f0771-d283-405b-90c0-eb6ed2262b52">

 		 
Selecting the docker repository as the default one

apt-cache policy docker-ce

<img width="855" alt="Screenshot 2023-05-25 at 00 20 37" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/fbb3fd4e-9e74-4a3f-bda4-8703a799f745">
 		 

Installing docker

sudo apt install docker-ce docker-ce-cli containerd.io

<img width="851" alt="Screenshot 2023-05-25 at 00 21 28" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/f339f102-c738-4c66-a35f-9458051225e3">


 		 
Checking service status

sudo systemctl status docker

<img width="859" alt="Screenshot 2023-05-25 at 00 22 00" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/8bbb5bd6-ac81-44cf-b774-c5b02587090e">
 		 


Adding the current user to the docker group to be able to run the docker command

sudo usermod -aG docker ${USER}

<img width="844" alt="Screenshot 2023-05-25 at 00 23 39" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/a8623651-5994-465f-8e99-72ce55772086">
		
 		 
Verifying the installation.

docker --version

docker version

docker info

docker container run hello-world

<img width="866" alt="Screenshot 2023-05-25 at 00 25 14" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/8c93a6fb-58b6-4752-b082-d0e099e7b4f6">

<img width="860" alt="Screenshot 2023-05-25 at 00 26 04" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/46f7e872-1424-4748-8fd8-59f81bcfe1aa">

<img width="793" alt="Screenshot 2023-05-25 at 00 49 02" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/133f6b5e-6765-4c9a-a3bb-65e3988853cc">
 		 



Getting help

Docker help

docker MANAGEMENT_COMMAND help => Ex: docker container help

<img width="817" alt="Screenshot 2023-05-25 at 00 49 57" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/29c1ac5b-7037-4c06-948a-3f73dac57155">

 		 
Searching for an image on Docker Hub

docker search IMAGE_NAME

docker search debian

<img width="1153" alt="Screenshot 2023-05-25 at 00 51 10" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/53bc861b-7345-422f-8ca5-6449f7223a86">


docker search mongo

<img width="1154" alt="Screenshot 2023-05-25 at 00 51 45" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/674b4708-6d08-4ffa-aa5f-109835099e51">
 		 
		 
Pulling an image from docker hub

docker image pull IMAGE_NAME:TAG

docker image pull redis:5.0.10

<img width="865" alt="Screenshot 2023-05-25 at 01 05 09" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/d9eba095-2a37-4d64-981c-fc985bbcb609">


docker image pull ubuntu:latest

<img width="862" alt="Screenshot 2023-05-25 at 01 05 53" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/8af3775f-d32e-4afd-9474-3e4835ec00ba">


docker image pull mysql # => by default the name of the TAG is latest

<img width="1068" alt="Screenshot 2023-05-25 at 01 08 27" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/026deb5e-369d-4b17-81a0-86b8f154c829">

 		 
Listing local images

docker images

docker image ls

<img width="828" alt="Screenshot 2023-05-25 at 01 17 18" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/571cb92c-bcb8-4407-8f0e-bb93d23853e9">
		
		
Running a container

docker container run OPTIONS IMAGE_NAME

docker container run -P httpd

<img width="883" alt="Screenshot 2023-05-25 at 01 22 19" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/045ca9c5-3740-445b-9cc0-8e11842947cd">

equivalent to:

docker container create -P httpd

<img width="851" alt="Screenshot 2023-05-25 at 01 22 45" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/56c338a4-bc22-4a6e-b782-a24cddd72454">


docker container ls -a # listing all containers

<img width="1142" alt="Screenshot 2023-05-25 at 01 23 47" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/f034b510-365a-4532-b45a-d8410c242a24">


docker container start CONTAINER_ID
		 
Getting a shell into a container

docker container run -it centos # to detach from the container without stopping it press: Ctrl + P + Q

<img width="893" alt="Screenshot 2023-05-25 at 01 30 35" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/368a5c7e-38a9-44bd-851b-e7e9f4acced7">

<img width="1111" alt="Screenshot 2023-05-25 at 01 30 44" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/56928631-9195-44e6-805b-5028e92c5407">



Running a Web Server in a Docker Container ##

docker container run -d -p 80:80 --name mysite1 nginx

docker container run -d -p 8080:80 --name mysite2 nginx

docker container run -d -p 8081:80 --name mysite3 nginx


<img width="1081" alt="Screenshot 2023-05-25 at 01 37 39" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/297680ce-b99d-438d-a043-bbd3dd9dcdb0">

		 
-d => detach and run in the background

--name => container name, must be unique, randomly chosen from a list if it's not given

-p X:Y => publish a container's port to the host

process listens on port Y in the container but is accessed on port X from the outside (LAN/Internet)

-P => publish all exposed ports to random ports
 		 
Listing local images

docker images # old command

docker image ls # new command
 		 
Listing all running containers

docker ps # old command

docker container ls # new command

<img width="1157" alt="Screenshot 2023-05-25 at 01 41 20" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/fe2c182a-1ce9-4788-9b32-74fa0b06f801">


-q => printing only the containers' ids

 		 
<img width="816" alt="Screenshot 2023-05-25 at 01 42 15" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/bd6d3702-60cd-46d8-930c-af7061f4c096">


Listing all containers (created, running, stopped)

docker ps -a

docker container ls -a

<img width="1157" alt="Screenshot 2023-05-25 at 01 45 26" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/e6f14fd4-119e-4a3c-8964-87e36ebe19a1">

<img width="1156" alt="Screenshot 2023-05-25 at 01 45 54" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/a226f05f-c9e4-48a9-8347-8eed96862285">

 		 
Filtering by status

docker container ls -a -f status=exited


<img width="1116" alt="Screenshot 2023-05-25 at 01 48 52" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/752884ae-1fda-4a05-a8f5-3eaddf0ce97f">

 		 

Stopping a container

docker container stop CONTAINER_ID|CONTAINER_NAME

Example: docker container stop mysite1
 		 
Removing a container (must be stopped)

docker container rm CONTAINER_ID|CONTAINER_NAME

-f => force remove the container (can be running)

Example: docker container rm mysite1
 		 

Removing all stopped containers

docker container rm $(docker container ls -f status=exited -q)


<img width="1150" alt="Screenshot 2023-05-25 at 01 50 58" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/ec4ad358-4ef5-46db-bcbd-6c10f6efd765">
 		 
Removing an image

docker rmi IMAGE_NAME # => old command

docker image rm IMAGE_NAME # => new command

Example: docker image rm nginx

<img width="1163" alt="Screenshot 2023-05-25 at 01 57 20" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/a3e82edc-e1d1-4bc5-99a7-8dc3aedb6877">

		 
Removing dangling images, stopped containers, dangling build cache and networks not used

docker system prune

<img width="1117" alt="Screenshot 2023-05-25 at 01 57 55" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/bb7d178d-0f00-4888-8dd8-bd4382990802">


-a => remove unused images, as well

Getting shell access to the container

docker run -it --name=container1 centos => press Ctrl+P +Q to exit the container without stopping it

<img width="1145" alt="Screenshot 2023-05-25 at 01 59 31" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/ce2d7d5d-92eb-4e55-afe5-3190452d9695">
 		 
Getting a shell in a running container

docker container exec -it CONTAINER_ID|CONTAINER_NAME bash

Example: docker container exec -it container1 bash
 		 
executing shell commands in running container

docker container exec CONTAINER_ID|CONTAINER_NAME COMMAND

Examples: docker container exec container1 cat /etc/shadow

<img width="977" alt="Screenshot 2023-05-25 at 02 03 53" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/d1a0e099-5b3b-425a-b61f-a2fba37af61d">


docker container exec container1 yum -y install nmap

<img width="1055" alt="Screenshot 2023-05-25 at 02 05 09" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/aecb9d2a-30bf-4bb1-9428-78ae3d36ae64">
 		 
Stopping a running container

docker container stop CONTAINER_ID|CONTAINER_NAME
 		 
Starting a stopped container

docker container start CONTAINER_ID|CONTAINER_NAME
 		 
Removing a stopped container

docker container rm CONTAINER_ID|CONTAINER_NAME
		 
Removing a running container

docker container rm -f CONTAINER_ID|CONTAINER_NAME

Commands - Committing Changes, Tagging and Pushing Images

Commiting changes in a container to a new image

STEP 1

start the container, get shell access, make any changes and exit

docker container run -it --name=container1 centos

STEP 2

create a new image from the modified container

docker commit -m "What did you do to the image" -a "Author Name" CONTAINER_ID|CONTAINER_NAME repository/new_image_name:tag

Example: docker commit -m "nmap installed" -a Christopher." container3 mamiololo/my_centos

<img width="1152" alt="Screenshot 2023-05-25 at 02 15 48" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/bbc06159-fdc8-4854-ad0e-f4d47ade2808">


STEP 3

start containers from the image

docker image ls

<img width="905" alt="Screenshot 2023-05-25 at 02 16 20" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/d22bd24d-36c6-4220-b8ff-5a3a0981c7c9">

docker container run -it mamiololo/my_centos

adding a new tag to an existing image

docker image tag nginx ddandrei/nginx:custom

docker image tag ddandrei/my_centos:latest mamiololo/my_centos:1.0

<img width="1085" alt="Screenshot 2023-05-25 at 02 17 07" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/e182dc8f-7ab0-4f2c-9ccc-0978ace1805e">

<img width="1119" alt="Screenshot 2023-05-25 at 02 20 56" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/5b3c3746-73a6-49f0-ae6b-648341b334e5">


docker image ls

<img width="866" alt="Screenshot 2023-05-25 at 02 21 17" src="https://github.com/Mamiololo01/Fundamentals_of_Docker/assets/67044030/09fb922c-9413-4951-a09a-d647a3679db5">


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
 		 
Listing all volumes

docker volume ls
 		 
Inspecting a volume

docker volume inspect mysite
 		 
Removing a volume

docker volume rm mysite

docker volume prune     # => remove all unused volumes
 		 
Starting a container with the volume

docker container run -d --name mywebapp -p 80:80 -v mysite:/usr/share/nginx/html nginx
























