# Let's Learn Docker!

A quick overview of Docker for the absolute beginner.

On Youtube Here: https://www.youtube.com/watch?v=fQORO9QEJN4 

## A Docker Overview

### What is Docker?

Docker is a technology which simplifies the complexity of delivering software applications.

### The Problem Docker Solves

Suppose you are a budding data scientist. You might be interested in running **JupyterLab** https://jupyter.org/ to see what all the buzz is about.

To get JupyterLab up and running on your laptop, you'll need to:

1) Download and install Python
2) Install JupyterLab using Pip

Seems simple enough, right? But what if you want to program in the R language in JupyterLab? Now you have to:

1) Download and Install Python
2) Install JupyterLab using Pip
3) Download and Install R
4) Install the R packages you like to use using CRAN.
5) Configure R for use in JupyterLab

The list is getting long and complicated. The more stuff you install the more you have to maintain and keep up-to-date. You also need worry that you might install something that will conflict with other applications which require Python or R on your computer. Yikes!

### Images and Containers

Docker solves this problem by:

1) Allowing you to pre-package an application's software dependencies into an **image**. The image is simply a file on disk which contains the steps required to build it.
2) Running the code contained in the image. This is called a **container**. A container is a running instance of an image.

So in a nutshell, the "Docker way" of solving this problem is to build an image from the setup steps then run the image as a container. Containers run in isolation from the rest of the stuff running on your laptop.

You can run multiple containers on the same computer. Those containers can be the same image or different images. The computer running Docker itself is called the **host**. Each container is isolated from the other running containers and from the host. If you choose, you can permit the containers to see each other.

### Advantages of Docker

By bundling your software into images and then running it as a container, Docker provides 3 distinct advantages:

- Images contain all the dependencies, so you won't need to install anything to run the application.
- Isolation of running containers means you won't need to worry about software conflicts.
- Reproducibility! If it runs in a container on *your* laptop, it will run in a container on *my* laptop, too. (Or on any machine for that matter.) This is the true power of Docker.

With Reproducibility you can:

- build your application on your computer and deploy it to the cloud with confidence it will work.
- craft some crazy machine learning model and easily share it with your colleagues who can run it for themselves.
- work as a team and not have to worry about the ramp-up time for your teammates.

### Pre Built Images on Docker Hub

Docker Hub https://hub.docker.com is a catalog of publicly available Docker images. This is a good way to get started, but you should exercise caution when running other people's code. Only stick to verified images or images from sources you know and trust.

The act of downloading an image is called a **pull**.

By default, Docker uses the public catalog Docker Hub, but you can also configure your own private catalogs for your images.

### Roll Your Own Image in a Dockerfile

Sometimes you need to build your own image. You might build the image from scratch or start with an existing image and add more software on top of it. 

Images are defined in a special file called a `Dockerfile`. This file defines the steps required to build the image. You can learn more about the docker file here: https://docs.docker.com/engine/reference/builder/ 

### Docker-Compose: Bundling Multiple Containers Together

If you use Docker long enough, it is inevitable that you will need to run more than one container as part of a single application. For example, to run the popular content-management system WordPress https://wordpress.org/, you need two containers. One for the database and one for the wordpress application itself.

To make this happen we have the `docker-compose` utility, which reads the `docker-compose.yml` file. This tool allows you orchestrate running multiple containers under a single command. Learn more at: https://docs.docker.com/compose/compose-file/ 

## Installing Docker

To install Docker on your Windows or Mac, you'll need **Docker Desktop**. To install on Linux, you'll need the **Docker Engine** (also known as **Docker Server**). 

To install, follow these instructions here: https://docs.docker.com/install/ 

## Docker Walk-Through

The best way to learn Docker and really understand its value is apply it to a real-world scenario. In this section we will walk through the docker commands necessary to get JupyterLab https://jupyter.org/  up and running.

Docker runs from the command line, so first you must open a terminal.

NOTE: Do not type the `$` with the command. The `$` is there to help you to easily identify the commands to type.

### Image Basics

1. Let's download the Jupyter Data Science Notebook image from docker hub: https://hub.docker.com/r/jupyter/datascience-notebook. Type the following:   
`$ docker pull jupyter/datascience-notebook`  
NOTE: It will take a bit for the image to download.

2. When it is finished, let's pull another image, this time we will use a **tag** to ask for a specific version of the `nginx` web server, version `1.13.8`. Type:   
`$ docker pull nginx:1.13.8`   
NOTE: to see all the available tags for an image, consult docker hub. Here are the tags for **nginx** images https://hub.docker.com/_/nginx?tab=tags

3. Let's take a look at the images we pulled locally. Type:  
`$ docker images`  
to list them. You should see the `jupyter/datascience-notebook` image with the `latest` tag, as well as the `nginx` image with the `1.13.8` tag.  You will also see when the image was created (not pulled), and the size of the image.

### Container Basics

4. Let's run the `jupyter/datascience-notebook` as a container. Type:  
`$ docker run -d jupyter/datascience-notebook`   
NOTE: The `-d` runs the container in the background and returns you to the command prompt. Without it, you must press `CTRL+C` to return to the command prompt, which stops the container.

5. Let's see what's running, type:  
`$ docker ps`  
You should see the image `jupyter/datascience-notebook` is not running as a container. Pay attention to the `NAMES` column in the output. The container name is auto-generated, from an adjective and notable scientist. e.g. `relaxed_edison`   
Also, you can see the TCP or UDP ports the application runs on. In this case, the image requires `8888/tcp` but the running container does not expose this port of outside access (more on this later).

6. When an application runs, it usually generates logs which will give you an idea if its working correctly as well as other useful information. To view the container logs, type:  
`$ docker logs <contaner_name>`   
Of course, you should replace `<container_name>` with the actual name of your container e.g. `relaxed_edison`

7. For the `jupyter/datascience-notebook` image, the last bit of the logs provides a link you can visit in your browser to use the application now running in the container. It should be noted this is specifc to this particular image, and how to actually access the running container will vary from image to image.   
Here is my log output. NOTE: (your token will be different).  

```
 To access the notebook, open this file in a browser:
        file:///home/jovyan/.local/share/jupyter/runtime/nbserver-6-open.html
    Or copy and paste one of these URLs:
        http://618ba56e7f1f:8888/?token=22bb12535b3a88513e43a271b3e80e8930f8a4344762d268
     or http://127.0.0.1:8888/?token=22bb12535b3a88513e43a271b3e80e8930f8a4344762d268
```

8. Let's stop this container and then start it with the port exposed. To stop a container, type:  
`$ docker stop <container_name>`  
Again, make sure to replace `<container_name>` with the actual name of your running container.

9. Now when you type:  
`$ docker ps`  
You will no longer see the running container. Yes the container is no longer running but it is still there. Type:  
`$ docker ps -a`   
and you will see it. The status of the container is `Exited` which means the container has stopped.   
If we wanted to, we could start this container back up with `docker start`. There is no point in doing that, however, because this container was not created properly.  

10. Let's remove the container. Type:  
`docker rm <container_name>`  
Again, make sure to replace `<container_name>` with the actual name of your stopped container. Then type the following to ensure the container is gone:  
`$ docker ps -a`  
NOTE: you can only remove a stopped container.

### Containers and Ports

1. Now that we have a better idea about how containers work, let's run this container exposing the necessary ports and also giving the container a name. It is good practice to name your containers. Type:  
`$ docker run -d -p 8888:8888 --name jupyter1 jupyter/datascience-notebook`

2. There are two new wrinkles in this command. `--name jupyter1` names the running container `jupyter1`. Second, `-p 8888:8888` tells docker to use host port `8888` to use to expose the service, running on port `8888`. The first `8888` is the host, then second is the service configured in the image. Let's inspect the running container, type:  
`$ docker ps`   
Notice the `PORTS` column now has something like this:   
`0.0.0.0:8888->8888/tcp`   
This means the docker host (i.e. your computer) is now listening on TCP port `8888`, and if you were to access the service on this port, you will be redirected to `8888/tcp` listening inside the container. This is how we expose the running services inside the container to the outside world.

3. Let's take a look at the logs again, type:  
`$ docker logs jupyter1`  
At the bottom of the logs, you will see a URL to open Jupyter once again. 

4. Copy the URL, and open web browser and paste it in the address bar. You should see the main page of the Jupyter application! 

### Containers and Volumes

1. By default containers are read only. None of the changes made in a running container will remain after the container is removed. You might consider this no big deal, but its actually quite a problem when you need use containers in a production scenario.   

Docker volumes were designed to address this issue. Volumes allow you to map a folder on the Docker host to a folder inside the container. This allows the container application to be used but any changes made are saved outside the container.

Let's recreate the `jupyter1` container to expose the current folder to the Jupyter application.

1. First we must stop the current container, type:  
`$ docker stop jupyter1`

2. Next we must remove the container. Type:  
`$ docker rm jupyte1`  

3. Next we run the image again but use a volume called expose the folder  `jupyter-data` to persist  user's folder in Jupyter `/home/jovyan/`. It should be noted that the directory inside the image will be different depending on the image itself. You will need to read the documentation to figure out which volumes you can expose. Type:  
`$ docker run -d -p 8888:8888 -v jupyter-data:/home/jovyan/ --name jupyter1 jupyter/datascience-notebook`

4. As an exercise left to the reader, verify files persist: Scan the docker logs for the URL, login to Juypter, save a file. Stop and remove the container, then re-run and verify your files are there!

5. Where is this `jupyter-data` volume? Type:  
`$ docker volume ls`  
to see all volumes. You should see `jupyter-data`

6. Where are the files located? Type the following to see that:    
`$ docker volume inspect jupyter-data`

NOTE: In the final part we will mount a local folder as a volume using `docker-compose` this works better on the Windows platform than the `docker` command by itself.

### Docker Compose: Orchestrating Containers

By now we've typed `docker run`, `docker stop` and `docker rm` multiple times. At this point you must be thinking "there's got to be a better way!" This how much more complicated this might be if you had 4 containers in your application instead of just one. 

This is the principle behind `docker-compose`. Docker Compose allows you to manage several containers, their ports and volumes under one single command. If your application needs more than one container or has a complicated setup, then compose is the tool for you.

The basic principle is to define your containers in a `docker-compose.yml` file. In docker compose terminology, the containers are called `services`. For each service, we define how it will be run.

Here's an example `docker-compose.yml` file to run Jupyter exposing port 8888 to the host and the current working directory as the volume for `/home/jovyan`.

```
version:  '3.1'

services:
  jupyter:
    image: jupyter/datascience-notebook
    ports: 
      - 8888:8888
    volumes:
      - ./data/:/home/jovyan/
```

To use services (i.e. running containers) within the Docker-Compose file:

`$ docker-compose up -d`  to create and run the container(s).  
`$ docker-compose ps` to view the status of the running continer(s).  
`$ docker-compose down` to stop and remove the running containers(s)  

