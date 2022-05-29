# Docker Compose
Docker compose is used to work with multiple docker containers at once. It gets configured with either YAML or JSON files, YAML more common. 

### YAML vs JSON

*YAML*
- Has a key-value pair system, similar to JSON
- Indentation based
- list and [] are the same
- Divided in maps (objects) and sequences (arrays)
```yml
my-sequence: [item1 ,item2]
my-other-sequence:
	- item3
	- item4
my-map: 
	a-nested-sequence: [item5, item6]
	another-nested-sequence:
		- item7
		- item8
	a-nested-map:
		nesty: 'a string'
	a-different-nested-map: { key: value } # Comment

```

*JSON*
```json
{
	"my-sequence":[
		"item1",
		"item2"
	],
	"my-other-sequence": [
		"item3",
		"item4"
	],
	"my-map": {
		"a-nested-sequence": [
			"item5",
			"item6"
		],
		"another-nested-sequence": [
			"item7",
			"item8"
		],
		"a-nested-map":{
			"nesty": "a string"
		}
	}
}

```

---

# docker-compose.yaml anatomy

`version: # your docker compose file type version here (string)
services: # define your separate docker services here
volumes: # optional: define any shared storage here (can be any of the 3 types: volume, bind mount, tmpfs)
networks: # optional: your config here`

### Example
Docker compose example to run both server and client in same node image
- Services are run in the order they're listed unless expressed otherwise
- By default docker looks for file called docker-compose.yaml, change this behaviour with -f flag
	+ `docker-compose up -f my-compose.yaml`
- Shut docker-compose
	+ `docker compose down`

```yml
version: '3' #Depends on docker version but 3 is fairly safe
services:
	#server
	api:
		image: node:16.15.0 # To build container from
		ports:
			- 3000:3000 # First local - second container's port
		volumes:
			- type: bind 
			  source: ./server # server is the folder in my machine
			  target: /code # folder in container
		working_dir: /code # starting directory as container is run
		command: bash -c "npm install && npm run dev" # bash command run as container is opened

	#client
	client:
		image: node:16.15.0
		ports:
			- 8080:8080
		volumes:
			- type: bind
			  source: ./client
			  target: /code
		working_dir: /code
		command: bash -c "npm install && npm run dev"
		#depends_on: # list services this relies on (for docker to organise startup)
			#- another-service


```




# Example web application setup
*Requires* express server using `pg` package to connect to a postgres database

`/webapp
    |- docker-compose.yaml
    |- /db
        |- seed.sql
    |- /server
        |- package.json
        |- package-lock.json
        |- server.js
`
- In server.js
		+ `const { Pool } = require("pg");
		   const pool = new Pool()`	
		+ Pool needs access to environment variables (?)
- In package.js
	+ include start script



### Interact directly with DB
- If you have psql installed
	+ `psql postgres://<user>:<password>@localhost:<mapped-port>/<db-name>`

- If you don't
	+ `docker exec -it <container_name> psql -U <pg-user> <db-name>`




