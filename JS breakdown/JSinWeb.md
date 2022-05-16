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

- <document>.querySelector('element/.class/#id') -> selects element matching request
- <document>.querySelectorAll('xxx') -> selects all matching element and returns them in an NodeList. Need to cast the NodeList into array with Array.from()

- <var>.innerHTML -> change content of var with string of choice, best if within tags ex <h1> ... </h1>
- <var>.textContent -> retrieve/change text content of section

- <var>.setAttribute('key', 'parameters') -> set attribute to variable, key(example id-type-style) parameters(whatever is inside quotes)
- <var>.removeAttribute('key') -> remove all attributes related to key

- <document>.createElement('type') -> create new element that can be appended to a parent element !!-> add onclick event boi! -> document.createElement(..)
- <var>.appendChild(element) -> append element in other element
- <var>.append(element) -> appends to section too but not sure if more for alerts and that
- <newname>.createTextNode -> creates text section to append to paragraphs when it's required adding some other things in between (ex need to add an anchor link so you sandwich it in between two textNodes)

- <var>.remove() -> removes element
- <var>.disabled = true

*Similar to event listener*
- <var>.onclick = (e) => {function} -> when clicked execute function *very useful* when adding new elements


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
## Inside addEventListener('x', (e)=>{})

- input -> input variable has to be targeted outside eventListener
	+ input.value -> returns the value inputted by user

- <forms>.preventDefault() -> stop default behaviour (example forms refresh/redirect pages, you can stop that)

- Using the e (event) from the callback we can taget parent element
	+ e.target.parentElement

### Form types
```
<form>
	<input type='text | checkbox | number | color | submit'> (submit requires value='text to show')
<\form>
```
- type='':
	+ text
	+ checkbox
	+ number
	+ color
	+ submit -> requires value='text to show'
	+ textarea -> requires rows=x columns=x 
	+ password -> is a text that hides characters

- placeholder="" => placeholder for text inside box

- required => keyword, requires field to be full before submission	

- value="" => default value in the box

- name="" => VERY IMPORTANT TO PASS INFORMATION TO BACKEND


*IN FORM*
Add linking to other page once submitted / none -> refreshes page -> e.preventDefault() in event listener
<form action="otherpage.html">

To stay on the same page block default behaviour

*NEED A LABEL FOR INPUT with for="input-id"*
<label for="input1">.... id="input1"<\label>





---

### Fetch - inside addEventListener('submit, (e)=>{}')

`fetch(url) //send request to url
	.then(response => response.json()) //convert server response to json
	.then(data => { 
		console.log(data) //log the data to see what's in it
		})`

- FETCH DATA FROM WEB (Recieve or send data) 



---


## Binding
Binding is connecting events listeners to elements. This takes time so you can wait until page is loaded before loading binding with:

- DOMContentLoaded -> checks only if HTML is loaded, useful as bindings wouldn't happen without
document.addEventListener("DOMContentLoaded", () => {//Start doing the rest now})

- Load -> Checks that HTML, styling and images have finished loading, not ideal
document.addEventListener("load", () => {//do stuff})

- Defer in the script tag in HTML

---


# Packages

When working in the web, 'require' keyword to import modules does not work so it needs a bundler to take care of all the file linking. Here comes watchify

npm i -D watchify

*To use two commands together install concurrently*

npm i -D concurrently

*Need also jest-environment-jsdom if working with DOM*

npm i -D jest-environment-jsdom


_Concurrently is used to run a server simultaneously, running a server allowes us to see and track requests to our webpage_

### Now you can run this with npm 'dev' script short
`"dev": "concurrently \"watchify ./index.js -o bundle.js\" \"python -m http.server\""` -> watchify checks index.js and collects everything in bundle.js



# Tricks

### How to add new elements based on numbering eg item-1, item-2 ...
```js
//Get all lis
const lis = document.querySelectorAll('li')
//get last id
const lastID = lis[lis.length -1].id 
// item-3
//Slice number from string + cast in number (slice-1 takes last char) in this case 3
const lastNumber = parseInt(lastID.slice(-1))
//Now declare next id
const nextNumber = lastNumber + 1

//Now you can add element properly
const newLi = document.createElement('li')
//add class parsing nextNumber as id index
```

### You should always have a favicon! (little icon before page name on tab)
Inside head link:favicon => set path




