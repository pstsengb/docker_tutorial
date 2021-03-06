# Docker_basic   <img style="vertical-align:middle;" alt="logo" src="https://www.docker.com/wp-content/uploads/2022/03/horizontal-logo-monochromatic-white.png" height="25px">
### Docker Introduce:
Docker separate 2 main parts "image" and "container":

 1.  image     -> like a template which pre-installing what you want to make an image or you can download image already made by others.
  
 2.  container -> choose image you want to be to start up a container that  you can use to do execute inside pre-installing application certainly you can                     stop container or Start multiple at the same time.
## Docker image and container command line:
* Popular used commands

```css
 image instruction:
    $ docker pull ubuntu:20.04			# you can go docker hub to find image which already made by others,here use ubuntu:20.04 image
    $ docker image ls -a                        # your all image list 
    $ docker images                             # your all image list
    $ docker image rm 'image name or ID'        # remove image,'image name or ID' please type NAMES or IMAGE ID's front four number ,which can be found in your image list
    $ docker image tag 'image ID' 'new name'    # rename your image
    $ docker run -it ubuntu:20.04 bash		# use ubuntu:20.04 image to create a container
    
  container instruction:
    $ docker container ls -a				# your all container list
    $ docker start 'container name or ID'               # start a container. 'container name or ID' please type NAMES or CONTAINER ID's front four number ,which can be found in your container list
    $ docker exec -it 'container name or ID' bash	# when you start a container,you can access this container
    $ docker stop 'container name or ID'		# stop what you want container 
    $ docker container rm 'container name or ID' 	# remove container,'container name or ID' please type NAMES or IMAGE ID's front four number ,which can be found in your container list
    $ docker cp 'PWD/file name' 'container name:/PWD'   # copy file('PWD/file name' mean host file loaction) to your container loaction('container name:/PWD' you want to put at container loaction)
    $ docker cp 'container name:/PWD' 'PWD/file name'   # copy container file to host(example: docker cp ABC_container:/tmp/ABC.bag ./)
```
> Docker's version 20.10

## Create an image:
### 1.create image step by step 
* Create a file(ex:docker_image_create) and create txt(ex:docker_image_txt) ,which write down waiting to be processed command ,example like below
```css
FROM ubuntu:20.04
RUN apt update
RUN apt install -y sudo
CMD ["bash"]
```
> which means :use ubuntu 20.04  ->execute apt update  ->install sudo  ->CMD ["bash"] always add this in end.

* Go to docker_image_create file,and RUN commond as below
```css
$ docker build -t 'create image name' -f 'docker_image_txt' .
```
> 'create image name' ->type you want to create image's name
> 
> 'docker_image_txt' -> type you have created txt ,which already write down waiting to be processed command
### 2.create image using already exists container
* When you already have a container and you want to use current container to create an image,you can use below command
```css
$ docker commit 'your container name'
```
> 'your container name' ->type your already have container name

## Create a container by using own parameter :
* First create a txt.bash which content like below ,and go to that file,execute bash(./ "your txt name".bash)
```css
  #!/bin/bash
  xhost +local:docker
  docker run -it \
     --privileged \
     --network=host \
     --env="DISPLAY" \
     --env="QT_X11_NO_MITSHM=1" \
     --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
      #--volume="host pwd:/client location" \
     --name="example_container_name" \
     ubuntu_and_ros:latest
```
> Code explain:
> 
> Row 2,6,7,8 :using for authorization of access X server 
> 
> Row 4 :using for authorization of host device ,5 using for authorization of host port
>
> Row 9 :type your want to create container name
> 
> Row 10 :choose your image template

## Auto create a container and execute command  :
* First create a yaml which content like below 
```css
 version: "3"
 services:
   example_container:
    image: example_ros_image:latest
    command: bash -c "cd /home && source devel/setup.bash"
    network_mode: host
    privileged: true
    environment:
      ROS_MASTER_URI: http://localhost:11311
    container_name: example_container_name
    volumes:
      - "/home/test/Desktop/robot_2d_simulation:/ws_ros"
```
> Code explain:
> 
> Row 3 :enter a name
> 
> Row 4 :choose your image template
>
> Row 5 :enter you want to execute command
> 
> Row 6 :setting network authorization 
> 
> Row 7 :setting host device authorization 
> 
> Row 8,9 :setting environment
> 
> Row 10 :enter your want to create container name
> 
> Row 11,12 :host data mount(/home/pstsengb/Desktop/robot_2d_simulation:) to container(/ws_ros)

* Second go to that file,run commond as below
 ```css
$ docker-compose -f 'docker compose .yaml' up
```
> after docker compose a container,you can start to use 
> 
 ```css
$ docker-compose -f 'docker compose .yaml' up -d
```
> different part with above is after docker compose a container and terminal will exit,and container will keep execute at backgound,you can start to use 
> 

 ```css
$ docker-compose -f 'docker compose .yaml' down
```
> if you want to exit container ,type as above 
> 
