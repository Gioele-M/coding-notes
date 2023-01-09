# Connecting multiple micro-services

### YAML files are used to handle multiple requests
- Has a key-value pair system, similar to JSON
- Indentation based
- list and [] are the same
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

```

*Compared to json*
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

# Standard schema for client-server hosting

*Router -> Controller -> Model <-> Database*


### Server entrypoint
The server root has the entry point to our code, this retrieves the requests from the front end and communicates back. 
In here there is:
- API declaration with express + cors
- Imports router modules

```js
const app = require('express')()
const cors = require('cors')
const dogRoutes = require('./router/dogs')

app.use(cors())

app.use('/dogs', dogRoutes)

module.exports = app
```



### Router
Router stores all endpoints for each route. It includes:
- Router declaration
- Imports controller modules

```js
const router = require('express').Router()
const dogsController = require('../controllers/dogs')

router.get('/', dogesController) // functions is imported from controller file 

module.exports = router
```



### Controller
In controller we do make an async function to feed data to endpoints

```js
const Dog = require('../models/dog')

async function index(req, res){
	try{
		const dogs = await Dog.all
		res.status(200).json({data: dogs})
	}catch(err){
		console.log(err)
		res.status(500).json({error: err})
	}
}

module.exports = index

```



### Model
Model interacts with data directly and exports its object w getters and setters for accessing data
```js
const { dogData } = require("../initdb")

class Dog {
    constructor(data){
        this.id = data._id
        this.name = data.name
        this.age = data.age
    }

    static get all(){
        return new Promise (async (resolve, reject) => {
            try {
                const dogs = dogData.map(d => new Dog(d))
                if (!dogs.length) { throw new Error('No doggos here!')}
                resolve(dogs)
            } catch (err) {
                reject(`Error retrieving dogs: ${err.message}`)
            }
        })
    }
}

module.exports = Dog;

```


---

## Docker compose
Used to run applications in different containers, in this exaple is just to run the servie API

Docker compose is created in a YAML file
Requires

- Version
- services:
	+ api: (Service name)
		* image: # (ex node:16.15.0)
			- to build container from 
		* ports: 
			- (ex - 3000:3000)
			- expose a container port to a local port
		* volumes:
			- (ex)
				+ - type: bind
				+   source: ./server
				+   target: /code
			- where the code is and how to access it (storage)
			- Source and target do not need '-' for a documentation matter, however need to be indented on the same level of type word (not type's dash '-')
		* working_dir: code	
			- starting working directory as prompt starts
		* command: (ex bash -c "npm install && npm run dev")
			- what to do once container has started


Run docker compose
`docker compose up`

Stop docker compose
`docker compose down` (?)



### Docker compose example to run both server and client in same node image
Services are run in the order they're listed unless expressed otherwise

```yml
version: '3' #Depends on docker version but 3 is fairly safe
services:
	#server
	api:
		image: node:16.15.0
		ports:
			- 3000:3000 #first local second container
		volumes:
			- type: bind 
			  source: ./server #server is the folder in my machine
			  target: /code #server in container
		working_dir: /code
		command: bash -c "npm install && npm run dev"

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

```
































