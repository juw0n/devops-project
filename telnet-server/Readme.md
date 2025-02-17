### CONTAINERIZING AN APPLICATION WITH DOCKER

A container is the running instance of an application based off a container image. Using containers provides you with a predictable and isolated way to create and run code. It allows you to package an application and its dependencies into a portable artifact you can easily distribute and run.
The container layer is created when the container starts, and all changes (such as writing, deleting, and altering existing files) will occur in this writable layer. 
Additionally, a few boundaries and restricted views known as namespaces and cgroups isolate the container from the rest of the Linux host. These are kernel characteristics that restrict what a container on a host can see and utilise. They also make OS-level virtualization a reality.
The main point to remember is that namespaces limit what a container can see, while cgroups limit what it can use.

#### Dockerfile
The Dockerfile contains the instructions that teach the Docker server how to turn an application into a container image.

The most popular instructions are shown below:
FROM: It must be the first command in the file and specifies the parent or base image from which the new image is to be built.
COPY: adds files to a location in the image filesystem from your current directory, which houses the Dockerfile.
RUN: runs a command within the picture.
ADD: copies new directories or files to a destination in the image filesystem from a source or a URL.
ENTRYPOINT: The entry point enables your container to function as an executable, which is equivalent to any Linux.
CMD: command-line program that accepts host arguments.
CMD gives the container a default command or set of parameters (it can be used with ENTRYPOINT).

To build and run the docker image created for this project, run the following:
To build from dockerfile
==> docker build -t devops-vm/telnet-server:v1 .
To list existing image
==> docker image ls
To list running and non-running container
==> docker ps
==> docker ps -a
To stop a container
==> docker container stop <container-name>
To run a command inside a container or interact with a container, as if you were logged in to a terminal session.
==> docker exec <container-name> <command>
To remove a stopped container
==> docker rm <container-name>
To remove a docker image
==> docker rmi <container-name>
==> To get low-level information about docker object from a container in json format
==> docker inspect <container-name>
To get the history record of a docker image
==> docker history <image-name or repository-name>
To get a real-time update on the resources a container is using (like top in linux):
==> docker stats --no-stream <container-name>
To see all the logs in a container
==> docker logs <container-name>

