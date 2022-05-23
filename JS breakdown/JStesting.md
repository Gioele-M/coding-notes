# Testing

---

## Export functions
In the exporting file
`module.exports = functionName`
If more than one export as object
- good practice `module.exports = {fnName: fnName, fn2Name: fn2Name, ...}`
- lazy one `module.exports = {fnName, fn2Name, ...}`

In the importing file
(Import in single object even if you have more than one function to test, you can access the different functions with the '.' notation)
`const functionName = require('./file_name');
functionName.fnName`



## NPM

### Initialise NPM
`npm init -y` -> y flag states yes to all questions asked further

### Install package
`npm install <package_name>` 
*shorthand* `npm i <package_name>`

### Set developer dependency
`npm install --save-dev <package_name>`
*shorthand* `npm i -D <package_name>` -> Need jest + web stuff if any

### Add testing flags to npm json file
`"scripts : {"test": "jest -- watchAll" "coverage": "jest --coverage"}`

### Run the script
`npm test` or `npm run test`


## Random: publish NPM package
https://github.com/getfutureproof/fp_guides_wiki/wiki/Publishing-an-NPM-package

---

## Jest testing

### Declare test you want to run
Describe encoloses a set of tests

```js
describe('Overall testing description', ()=>{
	test('Specific test description', () => {
		expect(functionName(parameter)).toEqual(expected_return_value);
		})
	test('second test', () => {
		expect(fnName(x)).toEqual(y);
		})
	...
	});
```

Similar declaration taken from class (test and it should mean the same) ++ arrow function if passing a callback function

```js
describe('TestName', () => {
    it('SpecificTest', () => {
        expect(() => package.fnName()).toThrow(TypeError)
    })
})
```


### Common matchers in *JEST* -> expect(fn).<matcher>()
- .not.`<matcher>` -> inverts the operation 
- toEqual -> recursively checks every field in an object or array
- toBe -> tests *exact* equality
- toBeNull / toBeUndefined / toBeDefined -> check if object is null/undefined/defined (!undefined)
- toThrow -> checks if a function throws an error when called

*Numbers*
- toBeTruthy / toBeFalsy -> checks if returns boolean true/false
- toBeGreaterThan / toBeGreaterThanOrEqual -> checks if > / >=
- toBeLessThan / toBeLessThanOrEqual -> checks if < / <=
- toBeCloseTo -> used to check proximity of floats when rounding is required

*Strings*
- toMatch -> used against regular expressions

*Arrays and iterables*
- toContain -> checks if iterable contains a particular item

---

# Client side Dev Server

When we want to open a server to monitor a website we can do it with http.server
We might also want to bundle all of our code in a single file, this we can do with watchify
We use both these commands on the command line with concurrently

### Steps

- Setup in terminal
	+ npm init -y
	+ npm install -D watchify concurrently jest (jest-environment-jsdom -> for web development)

- Once npm is initialised paste this in "scripts"
	+ "dev": "concurrently \"watchify ./index.js -o bundle.js\" \"python -m http.server\""
	+ "test": "jest --watchAll"
	+ "coverage": "jest --coverage"

- Remember to link your HTML to JS (script src)

- Everything gets bundled in file bundle.js, remember to add that too!!

- Run server
	+ npm run dev
	
*ALL DONE!! UNLESS YOU WANT TO TEST TOO, IN WHICH CASE*

Useful creating a 'test file', in there put a layout.spec.js and add to top
Testing reactive functionality in jest requires two modules import

```js
/**
* @jest-environment jsdom
*/

const fs = require('fs') -> to interact w files
const path = require('path') -> to determine where files are at

//Build html
const html = fs.readFileSync(path.resolve(__dirname, '../index.html'), 'utf8');   //(one or two underscores before)

// YOU ALSO NEED TO SET THE DOCUMENT TO VALUE OF HTML STRING INSIDE EACH 'DESCRIBE' DECLARATION
beforeEach(() => {
        document.documentElement.innerHTML = html.toString();
    })
```
		

### Start testing!

Ex. Check if header is corerct

```js
describe('index.html', () => {

	// ADD some declaration before each test at top
	beforeEach(() =>{
	
	// example: insert here document declaration/extraction
	//Extract html content
	document.documentElement.innerHTML = html.toString()

	/*
	// !! You could use this instead but not too sure how 
    beforeAll(() => {
        body = document.querySelector('body')
    })
	*/
	
	})

	describe('head', () => {
		test('has title', () =>{
			//extract title
			const title = document.querySelector('title')
			expect(title.textContent).toContain('whatever title')
			})

	})
	describe('body', () ={
		it('has heading', () =>{
			let heading = document.querySelector('#heading')
			expect(heading).toBeTruthy()
			})
		})
})
```


# API testing
# Testing

- make __tests__ (underscores suggested in documentation) directory and name files `<file>`.spec.js or `<file>`.test.js (JEST REQUIREMENT) -> this case api.spec.js
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




























