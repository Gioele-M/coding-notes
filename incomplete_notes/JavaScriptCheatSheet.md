# *Javascript*

---

## Data variables
- var -> Function or global scope. Does not have to be initialized to a value
- let -> Block scoped. Does not have to be initialised to a value
- const -> Block scoped. Must be initialized to a value and cannot be reassigned
- no-declaration -> Similar to var goes to global scope

--- 

## Data types

### Primitives

- string -> `"Hello world"`
- number -> `1`
- boolean -> `true / false`
- null -> representing intentional absence of value
- undefined -> value assigned to newly declared variables
- symbol -> used to make unique identifiers
- bigint -> used to identify whole numbers larger than 2^53 -1 

### Complex

- array -> `['a', 'b', 'c', true, 123, null]` 
- object -> `{name: 'Zelda', age: 3, 'hair colour': 'blue'}`
- function -> Declaration function `function <functionName> (parameter) {return parameter*2}` (only function declaration which works with hoisting)
- Arrow notation function -> `x => x+3`
- Expression function `const <functionName> = function(){return xxx}`

---

Operators

- `+ - / *` -> add, subtract divide multiply
- `**` -> exponentiation
- `%` -> modulo - returns remainder of division
- `=` -> assigns value to variable
- `==` -> equal - returns true if both sides are equal (tries to convert to same data type before comparing)
- `===` -> strictly equal - returns true if values are **exactly** equal 
- `!` -> logical *NOT* eg. !true === false
- `&&` -> logical 'and'
- `||` -> logical 'or'

---
# If / Ternary / Switch

## If statements
`if(condition is true/truthy){
	//run code	
}else if(condition){
	//run code if condition true
}else{
	//run code if not met any condition
}
`
## Ternary statement
`condition ? code if true : code if false
age < 18 ? 'child' : 'adult'`

## Switch statements
`switch(variable){
	case <condition>:
		//do code if variable == condition;
		break;
	case <condition>:
		//do code;
		break;
	default:
		//code if no conditions are met
		break:
	}`

---
# Loops

## While loop
`while(condition is true){
	//run code
	//add a condition modifier or infinite loop
}`

## Do loop
`do{
	//code to run
	//same as before add condition modifier
}while (condition)`


## For loop
`for (let index=0; index </> condition; index++{
	//perform this code till stops
}`

---
# Iterators

## for...in (objects) / for...of (array)
Structure is the same but for...in is for objects and for...of is for arrays

`for(const key in object){
	//do this looping through keys
}
//
for(const element in array){
	//do this 
}
`

## forEach
forEach is used on arrays to apply a callback function (can establish an arrow function in it)
`array.forEach(element => console.log(element))`


## map
map is similar to forEach but it returns the values in a new array
`newArray = array.forEach(element => element.toUppercase())`


## some
some runs a callback function on the array until finds the element meeting the condition (not sure if can return anything)
`array.some(element => element.endsWith('x'))`


## filter
filter is similar to some but puts in a new array all the elements meeting the condition
`array.filter(element => element.endsWith('x'))`


## reduce (very tricky)
Basically it sums up (even if not numbers) all values in the array starting from the second argument and returns the total. If only 1 argument is given, it starts from 1st element of array. Other description:
It allows to keep a 'running total' of kinds (does not have to be numerical) and will pass it as an additional argument to the callback function to be operated on and returned. The final time, that running total becomes the return value of our reduce.
`let result = array.reduce((runningTotal, nextValue) => runningTotal + nextValue)`

## And many more...

---

# Functions

### 3 Types:
- In-built -> come with the language
- Libraries -> reusable functions collections
- Custom

### Declare and invoke
`function functionName(parameter, defaultParameter = 'xxx'){ //parameters are optional
	//code
	return toReturn //if any
}
functionName(argument)
`

### Declaration types
- Declaration function
`function functionName(){...}`
- Arrow function
`let functionName = () => {...} //brackets omitted if 1-liner`
- Expression function
`let functionName = function (){...}`

---

---

# JS in browser

## Link to HTML

In html script:
<script src="index.js"></script>
Either at end of body or in head with keyword defer


## Target an element and add event listener
`//Target element
const variable = document.querySelector('element/.class/#id)'
**
//add event listener to element
variable.addEventListener('event_type', (e)=>{
	//Complete processes
}`

Consider putting the event listener to a truthy if statement to avoid having random listeners when elements are not there
`if(variable){
	variable.addEventListener(...)
}`

If document.addEventListener(...) it refers to all document


---

## Interact with HTML elements
- document.querySelector('element/.class/#id') -> selects element matching request
- document.querySelectorAll('xxx') -> selects all matching element and returns them in an NodeList. Need to cast the NodeList into array with Array.from()

- <var>.innerHTML -> change content of var with string of choice, best if within tags ex <h1> ... </h1>
- <var>.textContent -> retrieve/change text content of section

- <var>.setAttribute('key', 'parameters') -> set attribute to variable, key(example id-type-style) parameters(whatever is inside quotes)
- <var>.removeAttribute('key') -> remove all attributes related to key

- <var>.createElement('type') -> create new element that can be appended to a parent element
- <var>.appendChild(element) -> append element in other element
- <var>.append(element) -> appends to section too but not sure if more for alerts and that

### Interact with CSS
use style.property
ex.
<section>.style.color = 'red';

otherwise classic one 
<section>.setAttribute("style", "color: red; font-weight: bold")

---

# Events
<section>.addEventListener("action", callback)


*Event types*
Many different ones but most important are 
- click
- keydown
- focus
- blur
- hover
- mouseenter
- mouseleave
- https://developer.mozilla.org/en-US/docs/Web/Events

---
## Inside addEventListener{}

- input -> input variable is created by default 
	+ input.value -> returns the value inputted by user
- <forms>.preventDefault() -> stop default behaviour (example forms refresh/redirect pages, you can stop that)



### Fetch - inside addEventListener('submit, ()=>{}')

`fetch(url) //send request to url
	.then(response => response.json()) //convert server response to json
	.then(data => { 
		console.log(data) //log the data to see what's in it
		})`




## Binding
Binding is connecting events listeners to elements. This takes time so you can wait until page is loaded before loading binding with:

- DOMContentLoaded -> checks only if HTML is loaded, useful as bindings wouldn't happen without
document.addEventListener("DOMContentLoaded", () => {//Start doing the rest now})

- Load -> Checks that HTML, styling and images have finished loading, not ideal
document.addEventListener("load", () => {//do stuff})

- Defer in the script tag in HTML

---






---

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
`const functionName = require('./file_name');`



## NPM

### Initialise NPM
`npm init -y` -> y flag states yes to all questions asked further

### Install package
`npm install <package_name` 
*shorthand* `npm i <package_name`

### Set developer dependency
`npm install --save-dev <package_name>`
*shorthand* `npm i -D <package_name>`

### Add testing flags to npm json file
`"scripts : {"test": "jest -- watchAll" "coverage": "jest --coverage"}`

### Run the script
`npm test` or `npm run test`


## Random: publish NPM package
https://github.com/getfutureproof/fp_guides_wiki/wiki/Publishing-an-NPM-package

---

## Jest testing

### Main script (to be tested <script>.js)
Export functions to be tested
`module.exports = functionName`

### Testing file (<script>.test.js)
Import statement for Jest env if testing web applications
```
/**
* @jest-environment jsdom
*/
```

Import functions to be tested (Import in single object even if you have more than one function to test, you can access the different functions with the '.' notation)
`const functionName = require('./file_name');`
Declare test you want to run
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
- .not.<matcher> -> inverts the operation 
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
	+ npm install -D watchify concurrently (jest-environment-jsdom -> for web development)

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
const html = fs.readFileSync(path.resolve(_dirname, '..index.html'), 'utf8');   //(one or two underscores before)

// YOU ALSO NEED TO SET THE DOCUMENT TO VALUE OF HTML STRING INSIDE EACH 'DESCRIBE' DECLARATION
beforeEach(() => {
        document.documentElement.innerHTML = html.toString();
    })
```
		

### Start testing!

Ex. Check if header is corerct

```js
describe('index.html', () => {

	// ++++++
	beforeEach(() => {
        document.documentElement.innerHTML = html.toString();
    })


	// ADD some declaration before each test at top
	beforeEach(() =>{
	// example: insert here document declaration/extraction
	#Extract html content
	document.documentElement.innerHTML = html.toString()
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



------------------------

Needs

- forms and apis -> wed afternoon
- mocking functions 
- mocking fetch requests



- Fetch and Async -> thur morning
- OOP -> thurs afternoon




