# Revision

<!-- ## Target an element and add event listener
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
- <var>.textContent -> retrieve/change text content of section
- <var>.setAttribute('key', 'parameters') -> set attribute to variable, key(example id-type-style) parameters(whatever is inside quotes)
- <var>.removeAttribute('key') -> remove all attributes related to key
- <var>.createElement('type') -> create new element that can be appended to a parent element
- <var>.appendChild(element) -> append element in other element



---

## Inside addEventListener

- input -> input variable is created by default 
	+ input.value -> returns the value inputted by user
- <forms>.preventDefault() -> stop default behaviour (example forms refresh/redirect pages, you can stop that)



## Fetch - inside addEventListener('submit, ()=>{}')

`fetch(url) //send request to url
	.then(response => response.json()) //convert server response to json
	.then(data => { 
		console.log(data) //log the data to see what's in it
		})`

---
 -->


 
# Work with testing

### Required at head of document to make it work
```js
const fs = require('fs')
const path = require§('path')
const html = fs.readFileSync(path.resolve(__dirname,'../index.html'), 'utf8')

//Tests using describe/test etc

```


---------------------------------------------------


# Actual lecture

letentflip -> js tool -> no ES6 syntax (no arrow function + needs ';')

### Timeout functions
`setTimeout(function timer() {
	//execute code;
}, 8000); //Delay in ms eg 8s
`

# Promises

Promises have 3 states before producing a 'response'
- Pending
- Fulfilled
- Rejected

You use promises to perform actions in asynchronous manner


### Old format of fetching
```js
const fakeRequest = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (Math.random() < 0.5) {
        reject({ status: 404 })
      } else {
        resolve({ status: 200 })
      }
    }, 2000)
  })
}

fakeRequest()
  .then((res) => {
    console.log(res)
    console.log('request worked')
  })
  .catch((response) => {
    console.log(response)
    console.log('request failed')
  })

```

## fetch request structure
```js
fetch('url')
  .then(resp => resp.json()) 
  .then(data => {
		console.log(data)
	})
  //Then makes it wait until response from url was recieved 
  .catch(err =>{
		console.log(err)
	}

```


# async
Async always returns a promise

## await
- Usable only in a function that uses async
- Will pause the execution of the function until async has had the chance to return the promise
- Similar to writing then in fetch request

## async request structure
!need a try catch block
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

## async explained in buger functions (?)

```js
function orderBurger(makeBurger){
	//promise made here
	return new Promise(makeBurger)
}

let specialSauce=3
function makeBurger(resolve, reject){
	//takes time, executor
	if(specialSauce >0){
		specialSauce--
		resolve()
	}else{
		reject()
	}
}

function eatBurger(){
	//works on fulfillment
	console.log('yum burger')
}

function complain(){
	//works on rejection
	console.log('where is my burger')
}

function doSomethingWhileWaiting(){
	console.log('Doing something else..')
}


// DOING IT WITH FETCH
function placeOrder(){
	orderBurger() //promise request, needs to be fulfilled before next call of function so
		.then(eatBurger) //No brackets cus callback
		.catch(complain) //If no burger arrives complain
}

// DOING IT WITH ASYNC AWAIT
async function asyncAwaitOrder(){
	try{
		await orderBurger()
		await eatBurger()
	}catch{
		complain()
	}
}


for(const customer of customers){
	placeOrder()
	doSomethingWhileWaiting()
}

```











