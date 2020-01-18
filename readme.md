# Let's Learn Docker!

A quick overview of Docker for the absolute beginner.

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

1. Let's download the Jupyter Data Science Notebook image from docker hub: https://hub.docker.com/r/jupyter/datascience-notebook. Type the following:   
`docker pull jupyter/datascience-notebook`

2. It will take a bit for the image to download, when it is finished, let's run a container from the image by typing:  
`docker run -d jupyter/datascience-notebook`  
NOTE: The `-d` runs the container in the background and returns you to the command prompt. Without it, you must press `CTRL+C` to return to the command prompt, which stops the container.

3. Let's see what's running, type:  
`docker ps`  
To view all the running containers. You should see it up.

4. 


docker hub

`docker pull <image>`

`docker run <image>`

`docker run -d -P <image>`

`docker run -d -P --name jupyterlab <image>`

`docker stop jupyterlab`

`docker rm jupyterlab`

volumes

` -v .:/home/jovyan/work`

`docker-compose`

