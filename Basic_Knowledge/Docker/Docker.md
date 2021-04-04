# Theoretical Knowledge

[TOC]


## concepts

### virtual machine

In a nutshell, a virtual machine is a virtual application of computer system, which means it provides functionality that is needed to execute the operating system.

<img src="img/the_progress_of_virtual_machine.png" style="zoom:50%;" />

- traditionally, OS deployed on Hardware directly
- **Hypervisor**: In virtual machine, here is a hypervisor, which create and run virtual machine. VMware, Virtual Box are hypervisors. 
  - Hypervisor provides each VM a Guest OS and transform resource and request from VM to the hardware and back. 
  - Hypervisor optimize the usage of resources such as CPU cycles, or memory.
- Each VM is a atomic entity, with all the binaries and libraries required to run multiple applications.
- **Disadvantage**:
  - Hypervisor need to collect all requests and instructions from guests and translates them for the host OS, which costs additional time.
  - need more time to spin up and pull down.
- **Types** of Hypervisor:
  - Type 1: run directly on the system hardware. so it is completely independent of the host operating system.
  - Type 2: run on the host system, problem in the host system will affect any guest OS. VMware workstation and Virtual Box are Type 2 hyper.

### Container

container could be treated as a software that packaged up executable code and its dependencies or necessary libraries so that application can be ran on multiple different computing environment.

- **Image**: all these necessary libraries, tools and settings can be packaged into a static container image, which can be an active container in the run time.
- **name space**: container use name space to isolate resources between two containers. Name space enables **Linux processes** to be isolated in their own system environments without interfering with each other.
- it provides an OS-level virtualization.

### difference between VM and container

check this diagram.

<img src="img/VMs-and-Containers.jpg" alt="container architecture" style="zoom:50%;" />

Here are some differences:

1. there is no need to use the **hypervisor** layer under the container. Container will virtualize the Host OS and build on it directly.
2. there is no **guest OS** in container.
3. container can run faster and more efficiently.
4. they use **namespace** to isolate. namespace is a Linux kernel construct that can restrict where the application code can run on the system, it can include shared libraries which can be accessible by multiple containers.
5. VM can contain multiple applications but container can just support one application.

### `Docker`

A `container` runtime to create `container`.  a `implementation` of `container runtime interface`

### `Docker Daemon`

The **`Docker daemon`** ( `dockerd` ) listens for **`Docker`** API requests and manages **`Docker`** objects such as images, containers, networks, and volumes. A **`daemon`** can also communicate with other **`daemons`** to manage **`Docker`** services.

### `Docker image`

A **`Docker image`** is a read-only template that contains a set of instructions for creating a container that can run on the **`Docker`** platform. It provides a convenient way to package up applications and preconfigured server environments, which you can use for your own private use or share publicly with other **`Docker`** users.

[reference here](https://jfrog.com/knowledge-base/a-beginners-guide-to-understanding-and-building-docker-images/#:~:text=A%20Docker%20image%20is%20a,publicly%20with%20other%20Docker%20users.)

## comparison

### different between VM image and docker image

1. **Snapshot process is faster in Docker than VMs**

   We generally start with a base image, and then make our changes, and commit those changes using docker, and it creates an image. This image contains only the differences from the base. When we want to run our image, we also need the base, and it layers our image on top of the base using a layered file system. File system merges the different layers together and we get what we want, and we just need to run it. Since docker typically builds on top of ready-made images from a registry, we rarely have to "snapshot" the whole OS ourselves. This ability of Dockers to snapshot the OS into a common image also makes it easy to deploy on other docker hosts.

2. **Startup time is less for Docker than VMs**

   A virtual machine usually takes minutes to start, but containers takes seconds, and sometime even less than a second.

3. **Docker images have more portability**

   Docker images are composed of layers. When we pull or transfer an image, only the layers we haven’t yet in cache are retrieved. That means that if we use multiple images based on the same base Operating System, the base layer is created or retrieved only once. VM images doesn't have this flexibility.

4. **Docker provides versioning of images**

   We can use the docker commit command. We can specify two flags: `-m` and `-a.` The `-m` flag allows us to specify a commit message, much like we would with a commit on a version control system:

   ```bash
   $ sudo docker commit -m "Added json gem" -a "Kate Smith"
   0b2616b0e5a8 ouruser/sinatra:v2
   4f177bd27a9ff0f6dc2a830403925b5360bfe0b93d476f7fc3231110e7f71b1c
   ```

5. **Docker images do not have states**

   In Docker terminology, a read-only Layer is called an image. An image never changes. Since Docker uses a Union File System, the processes think the whole file system is mounted read-write. But all the changes go to the top-most writeable layer, and underneath, the original file in the read-only image is unchanged. Since images don't change, images do not have state.

6. **VMs are hardware-centric and docker containers are application-centric**

   Let's say we have a container image that is 1GB in size. If we wanted to use a Full VM, we would need to have 1GB times the number of VMs you want. In docker container we can share the bulk of the 1GB and if you have 1000 containers we still might only have a little over 1GB of space for the containers OS, assuming they are all running the same OS image.

[reference here](https://stackoverflow.com/questions/29096967/what-are-the-differences-between-a-vm-image-and-a-docker-image)

# Lab

We operate the `container`, which are created by `Docker image` 

## start - build, pull and run

The diagram of `docker build`, `docker pull` and `docker run`

![start docker](img/start_docker.jpg)

To create a `container`, you must have a local `image` to use (If not present, one will be pulled to your local system). Most `images` are stored on the public `Docker Hub registry` and are pulled automatically as part of the docker run command. Docker images can also be stored on private or other registries.

The most common and popular `registry` to use is [`Docker Hub`](https://hub.docker.com), a cloud-based repository where `Docker` users and partners create, test, store, and distribute `container images`. 

commands:

- ```bash
  docker run --name [Name_of_your_container] -d -p 80:80 [Image_name]
  ```

  - `-d`: Runs the `container` in detached mode leaving your current terminal free as well as allowing the `container` to run in the background.
  - `-p`: Specifies the ports. The number on the **left** is the port on the host machine (running `Docker`) and the number on the **right** is the port that will receive the traffic within the `container`.
  - step taken by docker
    1. Searched for the Nginx image locally and couldn't find it.
    2. Pulled the Nginx image from Docker Hub.
    3. Docker daemon created a container from that image.

- to pull a certain image to create the container. `docker pull nginx`.

- to view all of the `images` stored on the local system: `docker images`

- use [docker hub](https://hub.docker.com) where Docker users and partners create, test, store, and distribute container images.

- check the connect: `curl <ipOfYourHost>:<port>`

## stop and start

```shell
docker stop [container name or ID] # stops a running container
docker start [container name or ID] # restarts a container
docker ps
docker ps -a
docker start [container name or ID]
```



## check the containers

- `docker ps --format "table {{.Names}}\t{{Image}}"`

## go into `container`

Normally with a virtual machine you would use a GUI or SSH to access and control it. With containers, things are a little bit different. Docker allows you to trigger an interactive shell into the environment within a container and manipulate it through that.

- create a interactive session with the container `docker exec -it [name of container] /bin/bash`

  - **-it**: Launches interactive shell.
  - **name**: The container you would like to enter, you can also put the container ID here.
  - **/bin/bash**: The shell interpreter to be used.

- copy file into the container

  ```shell
  # Format = docker cp <source> <destination>
  docker cp training-html/. <containerName>:/usr/share/nginx/html
  ```

  - `.`: copy the contents of the directory and not the directory itself

## network

Since `container` works on the host, we need to map the `host port` to `container port`.  When we run 

- ```bash
  docker run --name [Name_of_your_container] -d -p 80:80 nginx
  ```

actually we map the host's 80 port to our `container`'s 80 port, since the `nginx` listen to the 80 port (of our `container`), so it is inconvenient to change the `container`s port. We can change the host's port from 80 to 8080 by using the command below.

- ```bash
  docker run --name [Name_of_your_container] -d -p 8080:80 nginx
  ```

## commit and push

In this part we need to commit `container` to `image` to try to push `image` to the `Docker Hub`. don't confuse `container` and `image` here.

###  create account

 [`Docker Hub`](https://hub.docker.com/)

### commit

After developing the software in the `container` you can package it up back to `Docker image` (commit) by the code below.

```bash
docker commit -m="any message you want" [container_name] [image_name]
```

view the `images` to check the new `image` has been generated: `docker images`

### login

`docker login`

### tag

When developing with `containers` it is important to tag your `images`. The convention you use to assign a tag is up to you; however if no tag is specified, the "latest" tag will be assigned by default. The same applies when requesting `images` - if no tag is specified, the latest tag will always be assumed. This means that if you create an `image` with the tag "blueVersion", you must specify that tag when you try and retrieve it. Otherwise, `Docker` will look for the "latest" tag.

```bash
docker tag [IMAGE_ID] [DockerHubUsername]/[DockerHubRepo]:[tag]
```

To get the `IMAGE_ID`, use `docker images` to find the `image` you just commit.

Then there are now two different "versions" (defined by tags) on your machine:

- One with the "latest" tag that was used by default when you created the image.
- One with the tag you just created.

### push

```bash
docker push [DockerHubName]/[DockerHubRepo]:[tag]
```

## remove the container

`docker stop [container_name]` first and then `docker rm [container_name]`.