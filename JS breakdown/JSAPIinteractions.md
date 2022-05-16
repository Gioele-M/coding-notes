# JS and APIs

*see ES5 formatting in lap1-week1-thursday-morning*
API = application programming interface (?)
Used to recieve and send data - Usually in json format

ex. of API address
api.github.com/users/username -> returns data on specified username


### Promises

Promises have 3 states before producing a 'response'
- Pending
- Fulfilled
- Rejected

You use promises to perform actions in asynchronous manner



### Retrieve data from API using fetch
```js
//Fetch will return a promise, .then keyword makes you add actions to perform on it
fetch(apiURL) //send request to url
	.then(response => response.json()) //convert server response to json
	.then(data => { 
		console.log(data) //log the data to see what's in it
		})
	// data parameters can be accessed with '.' notation ex data.student
	// The data is likely to be returned in an array of objects
```

_Example_: get github username's n of friends with fetch

```js
fetch(`api.github.com/users/${input.value}`)
	.then(response => response.json())
	.then(data => {
		//This creates the variable data which is similar to a json object
		console.log(data)
		//get number of public repos
		//data has attributes!
		console.log(data.public_repos)
		//set n of repos as h3
		h3.textContent = data.public_repos + ' public repos'
	})

	// !!!!!!!!!! CATCH ERRORS
	// This does only work when the network is down 
	.catch(err =>{
		console.log(err)
	})
```

### Send data using fetch()
!!! This requires an endpoint for posting!
```js
const url = "https://jsonplaceholder.typicode.com/posts"
const options = {
  method: 'POST',
  body: JSON.stringify({title: "Awesome Blog Post", body: "What a great day this was to make a new post", userId: 1})
}
fetch(url, options)
  .then(console.log("Posted post"))
  .catch(err => console.warn('Opa, something went wrong!', err))  
```
As well as 'GET' (default) and 'POST', we may also find endpoints which respond to the verbs 'PUT', 'PATCH' or 'DELETE'. Generally, 'PUT' is for when we want to replace something, 'PATCH' is for when we want to update one part of something and 'DELETE' is self-explanatory.


### Post data example
```js
// Example POST method implementation:
async function postData(url = '', data = {}) {
  // Default options are marked with *

  // You could put all this in a variable instead (neater)
  const response = await fetch(url, {
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, *cors, same-origin
    cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'same-origin', // include, *same-origin, omit
    headers: {
      'Content-Type': 'application/json'
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
    redirect: 'follow', // manual, *follow, error
    referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
    body: JSON.stringify(data) // body data type must match "Content-Type" header
  });
  return response.json(); // parses JSON response into native JavaScript objects
}

postData('https://example.com/answer', { answer: 42 })
  .then(data => {
    console.log(data); // JSON data parsed by `data.json()` call
  });
```













## Async
Considering that fetch cannot catch errors which aren't related to the network, it's best using async with a *try-catch* block

### async request structure
!need a try catch block
!need await keyword for it to pause the execution until function has completed
```js
async function getAsync(){
	try{
		const response = await fetch('url')
		const data = await response.json()
		console.log(data)
	} catch(error){
		console.log(error)
	}
}
```


_Example:_ get github username's n of friends wiht async
```js
async function getGitHubUserInfo(username){
	try{
		const url = 'api.github.com/users/' + username
		const response = await fetch(url)
		const data = await response.json()
		h3.textContent = data.public_repos + " public repositories"
	}catch(err){
		console.log(err)
		//create element
		const error = document.createElement('div')
		error.textContent = 'Oh no! ' + err
		error.className = 'error'
		error.onclick = closeError
		document.querySelector('header').appendChild(error)
	}
}
```


--- 

# Express
Express is a framework built on node 
- Used to build APIs
- NPM requires it so no -D -> server builderm
	+ npm install express --save


*Steps to work with Express*
- create separate directory
- npm init -y
- npm install express (need it so no -D)
- npm install i -D jest nodemon supertest concurrently(?) !!! This is for next steps
	+ (Need nodemon so to not restart the server to implement pages) 
	+ (supertest for creates abstract HTTP for testing)
	+ (concurrently runs parallel programs)
	+ (watchify is not required as we are using node which has no issues with 'require') 
- git init (if you want)
	+ need .gitignore with
		* node_modules/
		* coverage
		* .DS_Store (if on mac)
- Change .json
	+ "dev": "nodemon index.js", -> need to add more with concurrently
	+ "start": "node index.js", -> just not to keep typing it
	+ "test": "jest --watchAll",
	+ "coverage": "jest --coverage"


## File splitting

### app.js
app.js stores the declaration of server (express()) and the info/operations that are going to be composing the API. Exports app

! In here remember to declare endpoints for get, post, patch, etc...

```js
const express = require('express')
const app = express()
// If interacting with forms!
app.use(express.json())


//app.get(path, callback(eg request [what's asked], response [whats given]))
app.get('/', (req, res) => { 
// .get requires the page
	res.send('Hello Auguste!') //This is what's put on the page
}) 

//Export this
module.exports = app
```


### index.js ! file to be run
index.js imports the 'server' from app and gives it a port (usually 3000)

```js
const port = 3000 //Standard choice for development
const app = require(./app)

// Work in port->port and send 
app.listen(port, ()=>{
	console.log('Example in port ' + port)
})
```


---

# Testing

- make __tests__ (underscores suggested in documentation) directory and name files <file>.spec.js or <file>.test.js (JEST REQUIREMENT) -> this case api.spec.js
- For testing HTTP we need accessory package called supertest
	+ npm i -D supertest (can do with the rest above)


## api.spec.js
```js
const app = require('../javascript/app.js') //-> in here was imported express (server)
const request = require('supertest') // -> need to import it because not contained in the packages of jest when called in the command line

//Test example
describe('api server', ()=>{
	let api;
	//Perform this action before all tests
	//NEED AFTERALL TOO
	beforeAll(()=>{
		api = app.listen(5000, ()=>{  
		// Listen's first argument is referring to the port 
		// !!! Usually we use a different port because you can't run test and api in the same one (not too sure(?))
			console.log('Test server running on port 5000')
		})
	})

	test('It responds to get / with status 200', (done)=>{
		request(api) // Send request to api
			.get('/') // URL -> get this directory
			.expect(200, done) // Expect this works (200 signals it works, 404 that it doesnt etc)
	})

	//!!! *DONE* KEYWORD MAKES SURE THIS BLOCK OF CODE WAS COMPLETED BEFORE MOVING ON (manages asynchronous code)

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
			// ... is spread operator
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

---

## Testing workflow (CRUD)
- Create data
- Read data
- Update data 
- Delete data

### Default CRUD route
Route is what goes after the port eg localhost:3000*route* (*/* counts as route too)


### The 7 CRUD routes
*HTTP verbs are the methods that go after request AND when working with express in app.js (app.get(...))*
  	URL		  	HTTP Verb 		Action
- Read
	+ /cats 		get 		index 		(to see everything)
	+ /cats/:id  	get 		show		:dynamic (if there's > 1 index (:id=parameter))
- Create
	+ /cats/new 	get 		new			form -> usually displays a form AND interacts with backend
	+ /cats			post		create 		
- Update
	+ /cats/:id/edit  get 		edit 		form -> form per id
	+ /cats/:id 	patch/put 	update
- Delete
	+ /cats/id 		delete 		destroy	


*!!!!!!!!!!!!!!!!!!!!!!!!!!!!!*
In practice we swap post and get in create, this bc show is red before new and when typing .../new it will be interpreted as if we're looking for a page named 'new' instead of a form

---

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


---

# Differences between put and patch
- Put 
	+ PUT is a method of modifying resource where the client sends data that updates the entire resource .
	
- Patch
	+ PATCH is a method of modifying resources where the client sends partial data that is to be updated without modifying the entire data.



## Make PUT request using fetch

PUT is a method of modifying resources where the client sends data that updates the entire resource. PUT is similar to POST in that it can create resources, but it does so when there is a defined URL wherein PUT replaces the entire resource if it exists or creates new if it does not exist.
For example, When you want to update the Candidate name and email, you have to send all the parameters of the Candidate including those not to be updated in the request body, otherwise, it will simply replace the entire resource with the name and email.

```js
fetch(API_URL,{
	method: 'PUT',
	headers:{
	'Content-Type':'application/json'
	},
	body: JSON.stringify(DATA_WHICH_WE_WANT_TO_SEND)
})
```

### In async fn

```js
const putData = async ( ) =>{
   const response = await fetch('https://jsonplaceholder.typicode.com/posts/1', {
       method: 'PUT', 
       headers: {
         'Content-Type': 'application/json'
       },
       body: JSON.stringify(myDataObject)
   });
```




--- 


# PATCH
Unlike PUT Request, PATCH does partial update e.g. Fields that need to be updated by the client, only that field is updated without modifying the other field.

## Patch request method
```js
let PatchRequest = () => {
// sending PUT request with fetch API in javascript
fetch("https://reqres.in/api/users/2", {
	headers: {
	Accept: "application/json",
	"Content-Type": "application/json"
	},
	method: "PATCH",	

	// Fields that to be updated are passed
	body: JSON.stringify({
	email: "hello@geeky.com",
	first_name: "Geeky"
	})
})
	.then(function (response) {

	// console.log(response);
	return response.json();
	})
	.then(function (data) {
	console.log(data);
	});
};

PatchRequest();
```








---

# Interaction with backend and endpoint making

## Send JSON to backend
### in app.js 
(WHICH BASICALLY IS WHAT SENDS INFORMATION TO THE SERVER)
```js
//define objects/array you want to send
const cats = [{}]

//send to backend address <port>/cat
app.get('/cats', (req, res) => {
	res.send(cats)
})
```


## Send request based on data form 
### still in app.js

```js
// With this, using req.body you can see what was sent as request to the server
app.use(express.json())


app.post('/cats', (req, res)=>{
	console.log(req.body) //Just to see what the content of the request looks like
	
	//In this case we just want to append the information adopted: false to the request obtained in the server and send it 

	const newCatto = {... req.body, adopted:false}
	// you might want to push this in the array of data stored locally(?)
	// also add id from array.length +1

	data_array.push(newCatto)

	res.status(201).send(newCatto)  // -> send response code and data
	//
})
```


### Delete item
```js
app.delete('/cats', (req, res)=>{
	res.status(204).end() // 204 -> no content
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

--- 

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

---

---

# Hoppscotch -> check logic
https://hoppscotch.io/


## CORS -> blocks request from unknown users
Cross-origin resource sharing (CORS) is a mechanism that allows restricted resources on a web page to be requested from another domain outside local domain
Example play with hoppscotch 
_Requirements_

### in app.js

- install cors in command line
	+ npm install cors

- In app.js
	+ const cors = require('cors')
	+ app.use(cors())


---

# Deploy API with Heroku
- Requires dependencies
	+ cors
	+ express

## Standard script

### In index.js
```js
const app = require('./app')
// Declare port but don't override Heroku's one
const port = process.env.PORT || 3000

//Start server listener
app.listen(port, ()=> console.log('Express departed from port ' + port))
```

### Need a Procfile for Heroku to determine how to behave (no ext)
```
web: npm start
```
Means if run from web run npm start. Npm start is just 'node index.js'


### Deploy in CLI

- heroku login 
- follow what the website says for setup
	+ heroku git:remote -a dream-team-server (connects to their servers)
	+ heroku apps (shows all the working APIs)

- Now NEED to push IN MAIN!!! (wont work otherwise)
	+ git push heroku main
	!!!!! every time you change heroku somehow run this command!! Almost like heroku automatically opens a 'heroku' branch itself

YAY server up and running	

*Test server is running*  
heroku ps:scale web=1

*Open server from CLI*
heroku open

*See logs of activity*
heroku logs --tail

























## Remember!!
- Put beforeAll and afterAll when testing
	+ add *done* keyword
- it.only() runs only this test
- xtest() doesn't run this test
- !!! app.use(express.json()) in app.js to be able to read the form requests




