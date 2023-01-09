# Docker
Benefits
- Isolation -> isolate tasks
- Predictability -> create predictable environments
- Control -> control resources and easily scale

It creates containers which are runtime environments and work on kernels instead of having to establish virtual machines (?)

(Kernel establishes connections between applications and cpu-memory-devices)-(linux technically is a kernel)

{
Image: class
Container:instance of that class
}(?)

DockerHub 






### Docker local

Open dock app -> open terminal and type docker version
Client info are info regarding local machine
Server info are info on server where you build images and containers

*In cli*
*demo*

`docker pull hello-world` -> pull image

`docker run hello-world` -> run image + gives info on how to run image in this case
	- run is also shorthand for 
		+ `docker create + docker start`

*Other commands*

`docker ps` -> show all processes

`docker ps -a` -> should show all containers also if not running  (?)

`docker rm <container_id>` -> removes container

`docker images` -> lists all images -> Images are identified by tags

`docker rmi <image_id>` -> removes images -> can also do `<repository>:<tag>`

`docker run -it ubuntu bash` -> not too sure but should open bash in ubuntu (??????) -i interact <-> -t allocate a pseudo-tty (nice prompt(?))

`docker system prune` -> removes ALL containers and processes (doesn't touch the images)

`docker <any_command> --help` -> gives all options 

`docker start -i <container_id>` -> starts containter interactively


---


## Fresh start demo

```bash
mkdir demo
cd demo
touch index.js server.js package.json
code . (add all stuff for API)

docker run node:12.18.4
docker run -it --name my-api --mount type=bind,src="$(pwd)",dst=/code node:12.18.4 bash
```
-- name gives api a name
-- mount gives info for where getting info from/to and how to process (?) (NO SPACES IN BETWEEN) (fancy way of saying i want to add my code to the container)
	+ type = ?
	+ src = get content from (source key)
	+ dst = bring content to this ()
-- declare image you want to use (node...)
-- declare visualiser (bash)


---

Now youre in the interactive shell! install npm dependencies and run stuff


We install dependencies and all but it's an isolated container!!
Example:
	You're running an API but the localhost is not reachable!!
		NEEDS A -p flag in the long command! 

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


+++++++++
EXTRAS!!!

- -w /directory_name -> set working directory as processes initiates
- bash -c "npm install && npm run dev" -> script run as terminal/container is open

---

Can continue as before, you can do changes in vs code as usual and stuff would run as always refreshing and stuff.

NEED to npm i -> npm run xxx










