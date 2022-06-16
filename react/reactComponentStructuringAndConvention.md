# React component structuring

Component
- States -> data (helps describing our component)
- behaviour (all you can do with your component)

Example
Car
- states -> colour: red, petrol
- behaviour -> drive, fillTheTank


## Exporting



## Component example

For each of the components
- Make a js file and keep it inside src/components
- Requires import of React

```jsx
import React, {useState} from 'react'

const ReaderCount = ()=>{
	const [readsCount, setReadsCount] = useState(0)

	const increaseReadsCount = ()=>{
		setReadsCount(readsCount+1)
	}

	return(
		<>
		<p>There have been {readsCount} readers</p>
		<button aria-label="Read Story" onClick={increaseReadsCount}>I've read!</button>
		</>
	)
}

export default ReaderCount

```


## Form component example
```jsx

import React, {useState} from 'react'


const Greetings = ()=>{
	const [username, setUsername] = useState('friend')
	const [nameInput, setNameInput] = useState('User')


	//Two functions to handle form
	const handleInput = (e)=>{
		setNameInput(e.target.value)

	}

	const handleFromSubmit = (e)=>{
		e.preventDefault()
		setUsername(nameInput)
		setNameInput('')
	}


	return(
		<>
		<h3 aria-label='greeting' >Hi there, {username}</h3>

		<form role="" onSubmit={handleFromSubmit}>
			<label htmlFor='username'>Username</label>
			<input 
			type='text' 
			id='username' 
			placeholder="That's not my name" 
			value={nameInput}
			onChange={handleInput}/>
			<input type='submit' value="update"/>		
		</form>

		</>
	)
}


export default Greetings

```


## Important attributes

- *aria-label* attribute defines a string value that labels an interactive element -> Used for visually impaired ppl, their computer will read the label for the purpose of the button ex aria-label="Read story"

- *aria-checked* -> stores state of checkbox

- *htmlFor* -> same as for in HTML file (for is reserved in HTML so this is the work around)

- *value* -> input value

- *onChange* -> gets callback function for when button/whatever element is clicked

- *onSubmit* -> gets callback function for when form is submitted

- *role* -> attribute to reference for testing






# Props
Used to shared code between React components

*Card.js*
```jsx

import React from 'react'

function Card(props){
	return(
		<>
			<div>Hello {props.name}</div>
			<p>Your cohort was: {props.cohort}</p>
			<br />
		</>
	)
}


function App(){

	return(
		<div className="App">
			<header className="App-header">
				<Card name="Beth" cohort="Morris" />
				<Card />
				<Card />
			</header>
		</div>


	)


}


```

- *name* and *cohort* are considered props as they are storing values for the 'card' component to be injected later
	+ These are accessed with props.*name* and props.*cohort* from the calling object






---

# File system convention

*Divided in*
- Components
	+ All our components
- Pages
	+ To store all different redirections
- Layouts
	+ To hold components repeated frequently eg headers footers
	
- Tests file 
	+ Contains the setUp for tests 
	


*In each folder*
- Create an index.js file
	+ This is the file that exports all components

*Inside components*
- Create a folder per component, inside folder
	+ index.js (for the component)
	+ test.js (also specific to component) -> index.js is default import so just import './'

*Once created a component, in main component index.js file*
```js
export {default as FaveButton} from './FaveButton'
//Basically exports component to the app based on folder 
```


## Work with pages

*Make a folder per page*

*In page folder*
- Make index.js file
- Import React
- Export everything with
	```js
	export default = ()=>{
		...
	}
	```

*Again in the main index*
```js
export {default as News} from './News'

````


## Work with layouts

Same as before, index.js, file per component with index.js test.js files




