
# React component structuring

Component
- States -> data (helps describing our component)
- behaviour (all you can do with your component)

Example
Car
- states -> colour: red, petrol
- behaviour -> drive, fillTheTank


# Function naming convention
{on/handle}-Component-Action


# Component example

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


- *aria-label* attribute defines a string value that labels an interactive element -> Used for visually impaired ppl, their computer will read the label for the purpose of the button ex aria-label="Read story"




#### Other component

*FaveButton.js*
```jsx
import React, {useState} from 'react'

//Star-like checkbox
const FaveButton = ()=>{
	const [faved, setFaved] = useState(false)

	const handleFaveBtnClick = ()=>{
		setFaved(!faved)
	}

	return(
		<span style={{color: faved ? 'gold' : 'grey'}} onClick={handleFaveBtnClick} role="switch" aria-checked={faved}>
		★
		</span>

	)
}

export default FaveButton
```

- *role* -> attribute to reference for testing
- *aria-checked* -> stores state of checkbox



### Deal with forms
Controlled vs UnControlled forms

We'll work with controlled ones


Form for user greeting
*Greetings.js*
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

- *htmlFor* -> same as for in HTML file (for is reserved in HTML so this is the work around)
- *value* -> input value
- *onChange* -> gets callback function for when button/whatever element is clicked
- *onSubmit* -> gets callback function for when form is submitted
- *role* -> for testing again


*Components are then imported from App.js*
```jsx
import './App.js'
import ReaderCount from './components/ReaderCount'
import FaveButton from './components/FaveButton'
import Greetings from './components/Greetings'

const App = ()=>{
	return(
		<>
		<ReaderCount />
		<FaveButton />
		<Greetings />
		</>

	)
}


```







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






























