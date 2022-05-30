# Standard schema for client-server hosting

*Router -> Controller -> Model <-> Database*


### Server entrypoint
The server root has the entry point to our code, this retrieves the requests from the front end and communicates back. 
In here:
- index.html
- static/
    + css/
        * index.css(imports others)
        * modal.css
        * nav.css (for navbar)
    + js/
        * layout
        * modal
        * requests


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