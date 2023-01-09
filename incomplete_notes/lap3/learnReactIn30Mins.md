https://www.youtube.com/watch?v=hQAHSlTtcmY
# Reference elements in react

- useState
	+ creates state
- useRef
	+ creates reference to get user input
- useEffect
	+ persist things in local storage so not to lose when refreshing


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



## Best id randomiser

`npm i uuid`
`import uuidv4 from 'uuid/v4'`
