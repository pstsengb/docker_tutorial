# Docker_tutorial
Docker separate 2 main parts "image" and "container":

 1.  image     -> like a template which pre-installing what you want to make an image or you can download image already made by others.
  
 2.  container -> choose image you want to be to start up a container that  you can use to do execute inside pre-installing application certainly you can                     stop container or Start multiple at the same time.
## Docker image and container command line:
> This is the version 20.10
```
  image instruction:
    $ docker pull ubuntu:20.04			# you can go docker hub to find image which already made by others,here use ubuntu:20.04 image
    $ docker image ls -a                        # your all image list 
    $ docker images                             # your all image list
    $ docker image rm 'image name or ID'        # remove image,'image name or ID' please type NAMES or IMAGE ID's front four number ,which can be found in your image list
    $ docker run -it ubuntu:20.04 bash		# use ubuntu:20.04 image to create a container
    
  container instruction:
    $ docker container ls -a				# your all container list
    $ docker start 'container name or ID'               # start a container. 'container name or ID' please type NAMES or CONTAINER ID's front four number ,which can be found in your container list
    $ docker exec -it 'container name or ID' bash	# when you start a container,you can access this container
    $ docker stop 'container name or ID'		# stop what you want container 
    $ docker cintainer rm 'container name or ID' 	# remove container,'container name or ID' please type NAMES or IMAGE ID's front four number ,which can be found in your container list
```
## Docker how to create image:
* Create a file(ex:docker_image_create) and create txt(ex:docker_image_txt) ,which write down waiting to be processed command ,example like below
```
FROM ubuntu:20.04
RUN apt update
RUN apt install -y sudo
CMD ["bash"]
```
> which means :use ubuntu 20.04  ->execute apt update  ->install sudo  ->CMD ["bash"] always add this in end.

* Go to docker_image_create file,and RUN commond as below
```
docker build -t 'create image name' -f 'docker_image_txt' .
```
> 'create image name' ->type you want to create image's name
> 
> 'docker_image_txt' -> type you have created txt ,which already write down waiting to be processed command
