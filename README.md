# Docker and Kubernetes Exercises

A few Docker and Kubernetes Exercises for a beginner.


## Exercise 00: Setup

### Install and Configure Docker Desktop for Windows

1. Install Docker Desktop using the Software Center.
2. Is Docker running?
    - *Hint*: Look for the Docker icon in the notification area and right-click on it to get the menu.
3. Switch to Linux containers.
    - *Hint*: Right-click the Docker icon in the notification area and choose "Switch to Linux Containers".
4. Initialize the Kubernetes cluster.
    - *Hint*: Right-click the Docker icon in the notification area and look for "Kubernetes".


### Setup Git for Windows

1. Install Git for Windows.
	- *Hint*: Go to [https://git-scm.com/download/win](https://git-scm.com/download/win) to download the software.

### Familiarize Git Bash

1. Open a Git Bash.
    - *Hint*: Right click on a folder in your machine and see if you have a "Git Bash Here" shell command.
2. Can you navigate the file system?
	- What does `cd /` do?
	- What does `cd ~` do?
3. Can you manipulate the file system?
	- Try creating a directory
		- *Hint*: `mkdir`
	- Try creating a file
		- *Hint*: `touch`
	- Try removing the file you created
		- *Hint*: `rm`
	- Try removing the directory you created
		- *Hint*: `rmdir`
4. Create a directory structure with several files and remove it with a single command.
	- *Hint*:
        - **Warning**: This command can irrevocably damage your system. Make sure you are in the correct directory.
        - `rm -rf`


## Exercise 01: Our very first containers

### Exercise 01.1: Hello Docker

- Run a BusyBox container to print "Hello World"
- What did you see in the output?
- Run the same command again. Did the output differ? If so, why?

#### Hints

- BusyBox is a pre-built Docker image that runs a Linux box, available in the Docker Hub. Its image name is `busybox`.
- You can run any image by using this command:
    - `docker run imagename`
- You can provide a command to be run as well, in which case the syntax is:
    - `docker run imagename command`
- You can print "Hello World" in a Linux terminal like so:
    - `echo Hello World`


### Exercise 01.2: Containers with interactive terminal

- Run an Ubuntu container with an interactive terminal.

#### Hints

- Ubuntu is available in Docker Hub and its image name is `ubuntu`.
- `docker run` accepts flags.
    - `-it` is shorthand for `-i -t`.
    - `-i` tells Docker to connect us to the container's standard input.
    - `-t` tells Docker that we want a pseudo-terminal.


### Exercise 01.2: Install a software in our container

- Use the Ubuntu container and its interactive terminal from the previous Exercise.
- Try running `figlet hello`.
    - Is it available in your container?
- Looks like you need to install it.
    - How many packages were installed in your container before you installed figlet?
    - How many packages were installed in your container after you installed figlet?
- What happens when you restart the container and run the command again?

#### Hints

- In Ubuntu, as well as many other Debian based Linux distributions, software come in so called Debian Packages.
- `apt` is a command in Ubuntu that can manipulate packages installed.
- `apt update` updates the cache of available packages list.
- `apt install packagename` installs a package with the name `packagename`.
- `dpkg -l` lists the packages installed in our container.
- `wc -l` counts the number of lines in a file (or standard input, if a file is not given).
- `command 1 | command 2` in a bash shell runs the first command and pipes the output to the 2nd.


### Exercise 01.3: Run a non-interactive container

- Run image `sampathsris/clock`. What does it display?
- Terminate the container.

#### Hints

- Users can upload images to the docker hub, and then other users can use those images. Docker automatically downloads these images if you know the image's name.
- In this case, the image full name is required to run it with docker run.
- Image full name would be `username/imagename`.
- In Linux/Unix, you can usually terminate a program by sending `SIGINT` (Interrupt Signal). In command line, this is done by pressing `Ctrl^C`.


### Exercise 01.4: Run a container in the background

- Run the same container in the background.
- Do you see the output? Is the container running or terminated?
- Try to list the running containers. What information do you see?
- Spin up few more containers in the background and try to list them.
- Docker collects the output and logs it. Try to view the Docker logs.
- Try viewing the last few lines of logs.
- Try making docker show the last line of logs, and then keep on displaying new lines in real time.

#### Hints

- `-d` flag to `docker run` runs a container as a daemon (i.e. in the background).
- `docker ps` will list all the running containers and information about them, including the container ID.
- `docker logs <container_id/prefix>` will show the logged output of a container.
- See what `docker logs --tail 5 <container_id/prefix>` does. What happens when you change `5` to `10`?
- See what `docker logs –tail 1 --follow` does.


### Exercise 01.5: Stop or kill a container running in the background

- Stop a container that was started in the previous exercise.
- Kill a container as well.
- Did you say any difference on the time it took to terminate containers?
- Check if containers are still running.
- List all the containers which are running or terminated.

#### Hints

- `docker stop` and `docker kill` will help you. Both commands accept a container ID as an argument.
- We learned how to list running containers in the previous exercise.
- `docker ps` accepts a `-a` flag. See what it does.

 
## Exercise 02: Naming and labeling

### Exercise 02.1: Manipulating containers by name

- Start 3-4 containers from `sampathsris/clock`.
- Run `docker ps` to show their details.
- Without using the ID of the container, get logs and stop the containers.

#### Hints

- Most of the docker commands (such as `run`, `stop`, `start`) accept a parameter that identifies the container. This identification could either be,
    - The container ID: a random 256-bit integer, which is shown in the docker client as a 64-character hex string (e.g.: `d47d10bcd8c8bee5c96990dcccbf2d0edb3cada862102f1cadbf8932b7e2497e`)
    - Any unambiguous prefixes of the container ID above. (e.g. `d47d10bcd8c8`, `d47d`, `d4`).
    - The container name: a human-readable name provided automatically by docker for each container (e.g.: `festive_elgamal`). This is a combination of a randomly chosen adjective and a noun.
- You can also provide the container name with the `--name <container_name>` switch when you do `docker run`.


### Exercise 02.2: Container renaming

- Run `sampathsris/clock` container again but name it `my_clock`.
- Run the same image once again and try to give it the same name. What does happen?
- Rename the container named `my_clock` to `myclock`.

#### Hints

- `docker run` accepts a `--name <containername>` flag.
- See what `docker help rename` says.


### Exercise 02.3: Inspecting a container

- Use `docker inspect` to see details of a container.
- The output is too big. Try to use a Linux command to read it one screenful at a time.
- Print the created datetime and the image name of the container.

#### Hints

- Linux command `vim` and `less` could be used to read a file.
- You can also pipe output from other commands to `vim` and `less`.
- Output of docker inspect is a JSON object.
- You can pick and choose portions of this JSON object for output by using the `--format` flag to docker inspect.
- `--format` flag requires a format string. Within this string you can specify which properties of the object to choose by adding double curly brackets (`{{}}`).
- See the following guide for more info:
    - [https://docs.docker.com/engine/reference/commandline/inspect/#get-a-subsection-in-json-format](https://docs.docker.com/engine/reference/commandline/inspect/#get-a-subsection-in-json-format)
- Within this object you will find properties such as `Created` and `Config`. You will also find the property `Image` under `Config`.


### Exercise 02.4: Labeling containers

- Run `sampathsris/clock` with the label type set to value `alarm`.
- Run another one of `sampathsris/clock` with label type set to value `grandfather` (It's a grandfather clock, see?).
- Run yet another of `sampathsris/clock` with label type set to null (i.e., not the literal string "null", but a label with no value).
- Start few more containers for `sampathsris/clock` with no labels.
- Run `docker ps` but filter the containers that has the label `type`.
- Run `docker ps` but filter the containers that has the label `type` which has value `alarm`.

#### Hints

- Labels are used to add arbitrary metadata to containers.
- Labels are key/value pairs.
- They can be specified at container creation by specifying the `-l` flag.
