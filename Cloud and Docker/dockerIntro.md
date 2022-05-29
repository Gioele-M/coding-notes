# Docker
Docker is a platform set as a service products that use OS-level virtualization to deliver software in packages called containers.
Benefits
- Isolation -> isolate tasks
- Predictability -> create predictable environments
- Control -> control resources and easily scale

It creates containers which are runtime environments and work on kernels instead of having to establish virtual machines 
(Kernel establishes connections between applications and cpu-memory-devices)-(linux technically is a kernel)

## Docker elements
- Image -> represents what a class is in JS
- Container -> represents an instance of an image 

 
### Docker hello-word example

- Open dock app 
- open terminal and type `docker version`
	+ Client info are info regarding local machine
	+ Server info are info on server where you build images and containers
- *In cli*
	+ `docker pull hello-world` -> pull image hello-world
	+ `docker run hello-world` -> run image + gives info on how to run image in this case
	- run is also shorthand for 
		+ `docker create + docker start`



## Fresh start demo

```bash
mkdir demo
cd demo
touch index.js server.js package.json
code . (add all stuff for API)

docker run -it --name my-api --mount type=bind,src="$(pwd)",dst=/code node:12.18.4 bash
```
- --name -> gives api a name
- --mount -> gives info for where getting info from/to and how to process (?) (NO SPACES IN BETWEEN) (fancy way of saying i want to add my code to the container) (official:  When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its absolute path on the host machine)
	+ type = processing approach (bind mounts directory in host into container)
	+ src = get content from (source key -> working directory) (For bind mounts, this is the path to the file or directory on the Docker daemon host)
	+ dst = bring content to this (pre-established file where to save data) (The destination takes as its value the path where the file or directory is mounted in the container)
- declare image you want to use (node...)
- declare interface (bash)

Now youre in the interactive shell! Install npm dependencies and run stuff

*Example* script for exporting localhost address as well so to access it from browser in host machine 
```bash
docker run -it \
	--name my-api \
	-p 3000:3000 \
	--mount type=bind,src="$(pwd)",dst=/code \
	-w /code \
	node:12.18.4 \
	bash -c "npm install && npm run dev"
```

- -p -> share port, first port refers to the port i'm going to look for in my machine, second is the port declared in the container where output is 

- -w /directory_name -> set working directory as processes initiates

- node... is the image that initialises the container

- bash -c "npm install && npm run dev" -> script run as terminal/container is open

---

# *Useful Docker commands*

`docker ps` -> show all running containers

`docker ps -a` -> show all containers including if not running 

`docker rm <container_id>` -> removes container

`docker images` -> lists all images -> Images are identified by tags

`docker rmi <image_id>` -> removes images -> can also do `<repository>:<tag>` instead of id
	+ flag -f forces removal when there's a container associated

`docker system prune` -> removes ALL containers (doesn't touch the images)
	+ with -a flag deletes images too (unless there's at least 1 container in the image)

`docker <any_command> --help` -> gives all options 

`docker start -i <container_id>` -> starts containter interactively if not started already
	- -d in detached mode

`docker stop {container}` -> stops container execution

`docker exec [options] {container} {command} [arg...]` -> run a command in a running container
	- -i -> interactively
	- -t -> allocate a pseudo-TTY

`docker exec -it {container} psql -U postgres` -> opens prompt with postgres shell
	- psql should be the PostgreSQL interactive SQL command line
	- -U postgres is to run commands as the postgres user bc docker exec default refers to root user which does not have access to database

`docker attach {container_id}` -> attach a container to a running one

### Docker run flags

`docker run -it {image} {interface(bash)}` -> not too sure but should open bash in ubuntu 
- -i -> interactive  

- -t allocate a pseudo-tty (A device that has the functions of a physical terminal without actually being one, created by terminal emulators)

- run is shorthand for `docker create + docker start` 

- -e (POSTGRES_PASSWORD=password) -> set environment variable, in this case password (almost always required) (IN JS can retrieve this using!! process.env.{PARAMETER})

- -d {image} (postgres) -> run image in background and print container id (no interactive)

- -name names the container

- -w 

- --mount -> When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its absolute path on the host machine
	* type = processing approach (bind mounts directory in host into container)
	* src = for bind mounts, this is the path to the file or directory on the Docker daemon host
	* dst = the destination takes as its value the path where the file or directory is mounted in the container and data is saved

- -p -> share port, first port refers to the port i'm going to look for in my machine, second is the port declared in the container where output is 

- -w /directory_name -> set working directory as processes initiates

- bash -c "npm install && npm run dev" -> script run as terminal/container is open


---

### KILLER COMMANDS USE CAREFULLY!!!

| Command | Description |
|--- | --- |
| `docker kill $(docker ps -q)` | Stops all docker containers|
|`docker rm $(docker ps -a -q)`|Remove all docker containers|
|`docker rmi $(docker images -q)`|Removes all docker images|





---

# Make custom image

_in Dockerfile_
*Give a starting image*
FROM node:14

*Copy over any files you need*
COPY hello.js .

*Give a command to run upon the container starting*
CMD ["node", "hello.js"]

Then
`docker build .`
- -t {name} -> add name
- {name}:{tag} -> give specific, defaults to latest


## Make image from container

`docker ps -a`
To get container ID of container of interest then
`docker commit {container_id} {new_image_name}`


# Accessing local data in a container
You have 3 main methods

- *Bind* mount that points to a local host folder, you can access those files in your container, changes are immediately reflected and they remain in the container even when it is stopped. Volumes find host folders with absolute paths so make sure you are pointing the src to the full path eg: src="$(pwd)/myFolder where pwd is the print working directory command to get that full path.
`docker run -d \
--name bind-fetch-write \
--mount type=bind,source="$(pwd)"/output,dst=/output \
fetch-stuff`

- *Volumes* are similar to bind mounts but with some key differences. They find host folders with relative paths and they create a new folder within your Docker directories on your machine. These created folders are fully managed by Docker and you can interact with them via the Docker CLI.
`docker run -d \
--name volume-fetch-write \
--mount type=volume,source=output,dst=/output \
fetch-stuff`
`	docker volume ls: to list all
	docker volume inspect <volume-name>: to get info such as location
	docker volume rm <volume-name>: to remove volume
	docker volume prune: to remove all unused volumes`

- *Tmpfs* (temporary file system) does not persist the data. It allows access only for the life of the container. This is useful for sensitive information such as access codes.
`docker run -d \
--name temp-fetch-write \
--mount type=tempfs,dst=/output \
fetch-stuff`



# Push image to dockerhub

First log in 
`docker login`

Prepare image 
`docker tag <image-name>:<image-tag> <docker-username>/<image-name>:<image-tag>`

Push image 
`docker push <docker-username>/<image-name> `

Pull image 
`docker pull <username>/<image-name>`

