# Docker
Docker is a set of platform as a service products that use OS-level virtualization to deliver software in packages called containers.
Benefits
- Isolation -> isolate tasks
- Predictability -> create predictable environments
- Control -> control resources and easily scale

It creates containers which are runtime environments and work on kernels instead of having to establish virtual machines (?)
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
- --mount -> gives info for where getting info from/to and how to process (?) (NO SPACES IN BETWEEN) (fancy way of saying i want to add my code to the container)
	+ type = processing approach (bind mounts directory in host into container)
	+ src = get content from (source key -> working directory)
	+ dst = bring content to this (pre-established file where to save data)
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

- bash -c "npm install && npm run dev" -> script run as terminal/container is open

---

# *Useful commands*

`docker ps` -> show all running containers

`docker ps -a` -> show all containers including if not running 

`docker rm <container_id>` -> removes container

`docker images` -> lists all images -> Images are identified by tags

`docker rmi <image_id>` -> removes images -> can also do `<repository>:<tag>` instead of id

`docker run -it ubuntu bash` -> not too sure but should open bash in ubuntu (??????) 
	- -i -> interactive  
	- -t allocate a pseudo-tty (nice prompt(?))
	- run is shorthand for `docker create + docker start` 

`docker system prune` -> removes ALL containers and processes (doesn't touch the images)

`docker <any_command> --help` -> gives all options 

`docker start -i <container_id>` -> starts containter interactively


---


