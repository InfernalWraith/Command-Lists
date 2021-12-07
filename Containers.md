# Docker
## Basics
**Namespacing** is the act of isolating resources per process (or group of processes). Any time a particular process asks for a resource, it'll be directed to a specific area of a given space of hardware (such as the hard disk, or specific network port) reserved for it. A **control group** can be used to limit the amount of resources used per process (memory, CPU, HD I/O, network bandwidth). Namespacing and control groups are specific to the **Linux** OS, therefore programs which need these features (Docker being one of them) need you to host a Linux virtual machine in order to function properly.

The aim of **Docker** is to make the installation and execution of software as simple as possible - without worrying about setup or dependencies. Docker is a platform based around creating and running *containers*. An **image** is a single file with all the dependencies and configuration required to run a specific program. It's essentially a file system snapshot, but it also contains a startup command. A **container** is an instance of an image that can be thought of as a running program. It has its own isolated set of hardware resources (memory, networking, hard drive space). 

We interact with **Docker** by issuing commands to the **Docker Client** (**Docker CLI**), while the **Docker Server** is responsible for creating images, running containers, etc.

**Docker Hub** is a free public repository of docker images that can be downloaded and run. Once an image is downloaded, it is stored in the *image cache* for quick access.

The `run` command is used for creating and running a container from an image. Running the container calls the default command, but this behavior can be overridden if an additional command is passed as part of the `run`.
```dockerfile
docker run EXAMPLE_IMAGE
docker run hello-world

docker run EXAMPLE_IMAGE COMMAND_OVERRIDE
docker run busybox ping google.com
```


The `run` command is identical to running the `create` and `start` commands together. Creating a container is just preparing the file system snapshot for use, while starting it runs a command (the startup command by default). The `start` command can be used with the `-a` argument to collect output from the container and print it to the terminal.
```dockerfile
docker run hello-world OPTIONAL_COMMAND

docker create hello-world OPTIONAL_COMMAND
# A container ID is logged to the console
docker start -a CONTAINER_ID
```


Running containers can be stopped using the `stop` or `kill` command, with the container ID passed as an argument. `stop` shuts the container down with the `SIGTERM` message, which gives the internal process time to shut itself down and do some cleanup (a lot of programming languages can pick this message up to run cleanup or backup functions). `kill` issues a `SIGKILL` message, which shuts the container down immediately. If the container doesn't shut down within 10 seconds of the `stop` command, **Docker** will instead issue a `kill` command.
```dockerfile
docker stop CONTAINER_ID
docker kill CONTAINER_ID
```


A list of running containers can be acquired by using the `ps` command. The `--all` argument makes the command print out not only the currently running containers, but also those which were running earlier during the session and were stopped.
```dockerfile
# Print out currently running containers
docker ps

# Print out all containers which were run during this session
docker ps --all
```


Once a container which was stopped is run again, its startup command (not necessarily the default one, but the one we ran it with) runs and it can't be replaced. If we want to use other commands, we need to create a new container.

Disk space can be conserved using the `system prune` command. This command deletes all stopped containers, all networks not used by at least one container, all dangling images, and all build cache (images will have to be redownloaded from the **docker hub**).
```
docker system prune
```


If we want to get a container's output, but haven't passed the `-a` argument to the `start` command, instead of restarting and running the container again with the flag, we can use the `logs` command. This gets the record of all the logs that have been emitted from that container, without rerunning it.
```dockerfile
docker logs CONTAINER_ID
```


Additional commands in a container can be executed with the `exec` command. The argument `-it` can be passed to provide input to the container. `-it` is the same as typing in `-i -t`. `-i` attaches our terminal to the container program's `STDIN` (so we can pass input to the program), while `-t` ensures that all text passed to the program and displayed from it is formatted properly (also without it some autocomplete features might not work).
```dockerfile
docker exec -it CONTAINER_ID COMMAND

# Open a command terminal (shell) inside the container
docker exec -it CONTAINER_ID sh
```


When creating an image, we must create a **Dockerfile** - a plain text file that holds the configuration which defines how the container should behave. It will then be passed to the **Docker Client**, which then passes it to **Docker Server**. **Docker Server** will then build a usable image from the **Dockerfile**. Inside the **Dockerfile**, the following configuration items must be defined:
* Specify a base image
* Run commands to install additional programs (dependencies, etc)
* Specify a command to run on container startup