# React states

- *hook*
	+ hooks are functions that allow you to 'hook into' React state and lifecycle feature from function components
	+ hooks don't work inside classes, loops, conditions or nested functions (so they need to be declared at the top)


- *state*
	+ built-in React object that is used to contain data or information about the component
	+ you modify state when 'something' happens, such as event listeners
		* in react are called supported-events (look documentation)

*Covered*
- useState
	+ Creates storage and function to interact
- useRef
	+ Creates reference to get user input
- useEffect
	+ Used for fetching and storing data in localStorage



# useState
Creates a storage and a function to interact with that storage

*Counter example*
```jsx
import React, {useState} from 'react'

function Counter(){

	const [counter, setCounter] = useState(0)

	const handleBtnClick = ()=>{
		setCounter(prevCounter => prevCounter + 1)
	}

	return(
		<div>
			<h3>Count: {counter} </h3>

			<button onClick={()=> handleBtnClick()}>
				add one
			</button>
		</div>

	)

}

export default Counter

```




# useEffect with Axios

*Axios* is a substitute of fetch

`npm install axios`
Can be used both in node and browser

*Example*

```jsx
import './App.css'
import React, {useEffect, useState} from 'react'
import axios from 'axios'


function App(){
	useEffect(()=>{

		//Set useState
		const [students, setStudents] = useState([])

		//useState for input
		const [cohort, setCohort] = useState('')


		//useSTate for search term
		const [search, setSearch] = useState('')


		//State for handling errors
		const [error, setError] = useState('')

		//Status to communicate with the users
		const [statusMessage, setStatusMessage] = useState('Loading')



		//Given that useEffect does not take a async function as callback we need to make one inside

		const fetchStudents = async (searchTerm) =>{
			//On page reload, if there is no search term use that
			searchTerm ||= 'auguste'

			try{

				//URL declaration
				const URL = `https://raw.githubusercontent.com/getfutureproof/fp_study_notes_hello_github/main/${searchTerm}/roster.json`


				//This is the fetch request, returns a response
				const response = awaitaxios.get(URL)
				console.log(response)

				//can also straight destructure data from response
				const { data } = await axios.get(URL)
				console.log(data)

				//Set data as with useState function
				setStudents(data.students)

				//Empty the status message as not loading anymore
				setStatusMessage('')

			}catch(err){
				//set error
				setError(err)
				//Set status message
				setStatusMessage('Loading')
				console.log(err)
			}

		}

		//Set timeout for loading errors when calling app
		//Store ID in variable to 'clean after yourself'

		const timeoutId = setTimeout(()=>{
			fetchStudents(search)
		}, 3000)

		//Clean thing by

		return() =>{
			clearTimeout(timeoutId)
		}



	}, 
	[search]) //Called dependency array
	// Checks on values and runs if changes
	
	// Other cases for the dependecy array
	// empty -> runs in loop
	// [] -> runs once
	// [value, value2] -> will run every time the value changes (can have >1 variable)



	//Function to map students in lis
	const renderedStudents = students.map(st => {
		return <li key={st.github}>{st.name}</li>
	})//Keys are necessary for react to recognise elements



	//Function to handle input and set it as cohort of reference
	const onInputChange = () =>{
		setCohort(e.target.value)


	}

	//Function to handle form sumbit and preventing page refresh
	const onFormSubmit = (e)=>{
		e.preventDefault()
		//Set the cohort as value
		setSearch(cohort)
		//Clean cohort storage
		setCohort('')
	}


	return(
		<div className="App">
			<header className="App-header">
				//If there's error show to user, otherwise load status message and rendered students

				{ error 
					? <h1>Sorry, we could not find a(n) {search} cohort <h3>
					//else block if not error
					//if status message show it, after render students
					: <h3> {statusMessage ? statusMessage : ''} </h3> 
					<ul>
					{renderedStudents}
					</ul>
				}
 
				//If status message show it, otherwise dont
				<h3> { statusMessage ? statusMessage : '' } </h3>

				<ul>
					{renderedStudents}
				</ul>

				<form onSubmit={onFormSubmit}> //Callback for form submission
					<label htmlFor="cohort">Cohort</label>
					<input 
						type="text" 
						id="cohort" 
						value={cohort}//The value can be stored in cohort, previously declared in the useState
						onChange={onInputChange}//Function to handle input
						/> 
					<button></button>
				</form>

			</header>
		</div>
	)

}

```


























# Youtube video example including all states to create a todo list (no fetching)
*Todo.js*
Constains structure of todo
```jsx
import React from 'react'

//This has to import the functions and pass it in the elements
export default function Todo({todo, toggleTodo }){
	function handleTodoClick(){
		toggleTodo(todo.id)
	}

	return(
		<div>
			<label>
				<input type='checkbox' checked={todo.complete} onChange={handleTodoClick} />
				{todo.name}
			</label>
		</div> 
	)
}


```



*TodoList.js*
Contains mapping of todos elements
```jsx
import React from 'react'
import Todo from './Todo'

//This imports functions and passes down to mapped elements for reference
export default function TodoList({ todos, toggleTodo }){
	return(
		todos.map(todo => {
			return <Todo key={todo.id} toggleTodo={toggleTodo} todo={todo} /> //Key is for good practice
		})

	)
}

```




*App.js*
Main file
```jsx

import React, { useState, useRef, useEffect } from 'react'

function App(){

	//Use state initialised as empty
	const [todos, setTodos] = useState([])

	//Declare useref variable
	const todoNameRef = useRef()

	//---
	//Persist things when refreshing - first set a jey
	const LOCAL_STORAGE_KEY = 'todoApp.todos'

	//Render stored todos
	useEffect(()=>{
		const.storedTodos = JSON.parse(localStorage.getItem(LOCAL_STORAGE_KEY))
		if(storedTodos) setTodos(storedTodos)
	})

	//Save present todos
	useEffect(()=>{
		//pass things that you want to save and persist
		//Pass array of properties
		localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(todos))
	}, [todos]) //these are the items to persist
	//---


	// Function to add todo, takes user input and appends to list
	function handleAddTodo(e){
		const name = todoNameRef.current.value

		if (name === '') return

		setTodos(prevTodos =>{
			return[...prevTodos, {id: uuidv4(), name: name, complete:false}]
		})

	}


	// Function to handle checkbox of todo
	function toggleTodo(id){
		//create a copy of todo list
		const newTodos = [...todos]
		//find todo to target
		const todo = newTodos.find(todo=> todo.id ===id)
		//invert its checkbox
		todo.complete = !todo.complete
		//refresh todos
		setTodos(newTodos)
	}

	//Function to handle clearing of todos
	function handleClearTodos(){
		const newTodos = todos.filter(todo =>{!todo.complete})
		setTodos(newTodos)
	}


	return(
		<>
			<TodoList todos = {todos} toggleTodo={toggleTodo}/>
			<input href={todoNameRef} type='text' />
			<button onClick={handleAddTodo}>Add Todo</button>
			<button onClick={handleClearTodos()}>Clear Complete</button>
			<div>{todos.filer(todo => !todo.complete).length} left to do</div>

		</>


	)

}


```
