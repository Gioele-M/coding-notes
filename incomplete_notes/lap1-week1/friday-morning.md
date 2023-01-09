# Working with APIs and deplyoing

- Express -> framework based on node
	+ npm install express --save

*Steps to work with Express*
- create separate directory
- npm init -y
- npm install express (need it so no -D)
- npm install i -D jest nodemon supertest (Need nodemon this so to not restart the server to implement pages)
- git init (if you want)
	+ need .gitignore with
		* node_modules/
		* coverage
		* .DS_Store (if on mac)
- Change .json
	+ "dev": "nodemon index.js" 
	+ "start": "node index.js" -> just not to keep typing it
	+ "test": "jest --watchAll"
	+ "coverage": "jest --coverage"



- IN app.js
```js
const express = require('express')
const app = express()


//app.get(path, callback(eg request [what's asked], response [whats given]))
app.get('/', (req, res) => {
	res.send('Hello Auguste!') -> //This is what's put on the page
}) 

//Export this
module.export = app
```

- IN index.js
```js
const port = 3000 //Standard choice for development
const app = require(./app)

// Work in port->port and send 
app.listen(port, ()=>{
	console.log('Example in port ' + port)
})
```

- Now for testing 
	+ make __tests__ directory and name files <file>.spec.js or <file>.test.js (JEST REQUIREMENT) -> this case api.spec.js
	+ For testing HTTP we need accessory package called supertest
		* npm i -D supertest (can do with the rest above)

- IN api.spec.js
```js
const app = require('../javascript/app.js')
const request = require('supertest')

//Test example
describe('api server', ()=>{
	let api;
	//Perform this action before all tests
	beforeAll(()=>{
		api = app.listen(5000, ()=>{  // Listen's first argument is referring to the port 
			console.log('Test server running on port 5000')
		})
	})

	test('It responds to get / with status 200', (done)=>{
		request(api) // Send request to api
			.get('/') // URL -> get this directory
			.expect(200, done) // Expect this works (200 signals it works, 404 that it doesnt)
	})

	//DONE KEYWORD MAKES SURE THIS BLOCK OF CODE WAS COMPLETED BEFORE MOVING ON

	test('Gives 404 when trying removing a page', (done) =>{
		request(api)
			.delete('/cats')
			.expect(204, done)
	})


	// Test with it (which is the same as test really)
	it('responds to post with status 201', (done)=>{
		const testData = {
			name: 'xxx',
			age: 4
		}

		request(api)
			.post('/cats')
			.send(testData)
			.expect(201)
			.expect({...testData, adopted:false}, done) //get back test data and adopted needs to be false
			// doublecheck what ... was
			// I think it creates a copy and appends something to it
	})

	// it.only() currently tests only for this or xtest on stest to skip



	//Close test server after operation
	afterAll((done)=>{
		console.log('Stopping test server')
		api.close(done)
	})

})
```

## Testing workflow (CRUD)
- Create data
- Read data
- Update data 
- Delete data

### Default CRUD route
Route is what goes after the port eg localhost:3000/*route*

### The 7 CRUD routes

  	URL		  	HTTP Verb 		Action
- Read
	+ /cats 		get 		index
	+ /cats:id  	get 		show		:dynamic (if there's > 1 index (id=parameter))
- Create
	+ /cats/new 	get 		new			form
	+ /cats			post		create
- Update
	+ /cats/:id/edit  get 		edit 		form
	+ /cats/:id 	patch/put 	update
- Delete
	+ /cats/id 		delete 		destroy	






### Status codes
- 100 - 199 -> informational response 
- 200 - 299 -> successful response
	+ 201 -> Created
	+ 204 -> No content
- 300 - 399 -> Redirection messages
	+ 301 -> Moved permanently
- 400 - 499 -> Client error responses
	+ 403 -> Forbidden
	+ 418 -> I'm a teapot
- 500 - 599 -> Server error responses

### Status codes pages
- https://http.cat/
- https://httpstatusdogs.com/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status




## Send JSON to backend
IN APP.JS WHICH BASICALLY IS WHAT SENDS INFORMATION TO THE SERVER

```js
//define objects/array you want to send
const cats = [{}]

//send to backend address <port>/cat
app.get('/cats', (req, res) => {
	res.send(cats)
})
```

## Send request based on data form (?)
Still in app.js

```js
// With this, using req.body you can see what was sent as request to the server (req.body) 
//define req body
app.use(express.json())


app.post('/cats', (req, res)=>{
	console.log(req.body) //Just to see what the content of the request looks like
	res.status(201).send(... req.body, ...)
})
```

## Delete item
```js
app.delete('/cats', (req, res)=>{
	res.status(204).end()
})
```

## Give a page for each element ex cat1-cat2-... /cats/:id +++ 404 if out of index
```js
app.get('/cats/:id', (req, res)=>{
	//Get the request's parameter's ids (ids from the objects)
	const catId = parseInt(req.params.id)
	const cat = cats[catId - 1] 
	//cats is the array of objects
	//remember that indexes start from 0 so need -1
	res.send(cat)


	//You need to cover for indexes you're not using, eg if array.length = 5 and user goes to page 7 need to throw an error

	//Try-catch block with error throwing
	try{
		... //Here put code above starting from declaration of catId

		if(!cat){
			throw new Error('This cat does not exist')
		}else{
			res.send(cat)
		}


	}catch(err){
		res.status(404).send({message: err.message})
	}
}
	
```

### Structure test for this 'indexing' operation

Inside api.spec.js

```js
it('responds to an unknown cat id with a 404', (done)=>{
	request(api)
	.get('/cats/42')
	.expect(404)
	.expect({message: 'This cat does not exist'}, done) // We defined error message in other file
})
```


# Hoppscotch -> check logic
https://hoppscotch.io/


## CORS -> blocks request from unknown users
Cross-origin resource sharing(CORS) is a mechanism that allows restricted resources on a web page to be requested from another domain outside local
Example play with hoppscotch


### in app.js

- install cors in command line
	+ npm install cors

- In app.js
	+ const cors = require('cors')
	+ app.use(cors())







































