# Redux

## Demo

After initialising the app

- Create pages folder
	+ index.js file
		* exports pages (`export { default as ComponentName } from './ComponentName'`)
	+ 3 folders inside
		* Home
		* Deposit
		* Withdraw

- Import this in App.js
	+ Make sure you can navigate between different pages with
		* `npm install react-router-dom@6`
			- `import { Routes, Route } from 'react-router-dom'` 
			- Set routes (enclosed in `<Routes />` tag `<Route path="/" element={ <Home /> } />`

- Make nav link component in layout/Header
	+ `nav>ul>li*3`
	+ li: `<li><NavLink to="/">Home</NavLink></li>`
	+ Remember to make main layout index.js for export


- in main index.js
	+ `import { BrowserRouter } from 'react-router-dom'`
	+ Wrap `<App />` in BrowserRouter tags



# Share states between pages
*Redux*
Predictable states container
https://redux.js.org/tutorials/essentials/part-1-overview-concepts#terminology
`npm i redux react-redux`

- in main index.js
	+ `import { Provider } from 'react-redux'`
		* Used to pass info down to components
	+ `import { createStore } from 'redux'`
		* Deprecated command
		* Creates store with action to interact with variable
	
```jsx
import reducer from './reducers/reducer'

const root = ReactDom.createRoot(document.getElementById('root'))

const store = createStore(reducer)

root.render(
	<>
		<Provider store={store} >
			<BrowserRouter>
				<App />
			</BrowserRouter>
		</Provider>

	</>

)

```



# Build reducer

- New folder reducers
	+ reducers.js file
	
```jsx

const initialState = {
	balance:0
}

const reducer = (state=initialState, action) =>{

	switch (action.type) {
		case "DEPOSIT":
			return {..., balance: state.balance + action.payload}
			break;

		case "WITHDRAW":
			return {..., balance: state.balance - action.payload}
			break;

		default:
			return initialState;

	}

}
export default reducer;


```

- action is a js object that takes a type



*Get data from reducer*
*In components files, this case home.js*

```jsx

import React from 'react'
//let us read the redux state
import { useSelector } from 'react-redux'

const Home = () =>{
	
	const balance useSelector(state => state.balance) //retrieve only balance 

	return(

		<h1> balance: {balance}</h1>
	)
}

```

*Interact with data*
```jsx
import React from 'react'
//let us read the redux state / let us dispatch commands
import { useSelector, useDispatch } from 'react-redux'

// You can do the same for Withdraw

const Deposit = () => {

	// useSelector to retrieve redux value 
	const balance = useSelector(state => state.balance) //Once you implement second reducer, you need to call state.reducerName.reducerVar 
	// ex. state.balanceReducer.balance

	// useDispatch to interact with redux value
	const dispatch = useDispatch()

	const handleDeposit = () =>{
		dispatch({type: "DEPOSIT", payload: 100}) // payload hardcoded for a button matter
	}

	return(
		<>
			<h1>
				balance: {balance}
			</h1>

			<button onClick={handleDeposit}>
				Add 100
			</button>
		</>
	)

}

```



## Add 2nd reducer

Need additional package to do that

`npm install`


Previous reducer needs to implement changes too (name/filename)

Usage is the same

*reducers/loanReducer.js*
```jsx

const initialState = {
	loan:false
}


const loanReducer = (state = initialState, action) =>{
	switch (action.type) {
		case "APPLY":
			return { loan: true }
			break;

		default:
			return state 

	}
}

export default loanReducer

```



## >1 reducers
To use >1 reducer, you need to apply changes to the main index.js, implementing combineReducers
*main index.js*
```jsx
// all other imports
import { createStore, combineReducers} from 'redux'


const store = createStore(combineReducers({
	balanceReducer, loanReducer

}))


```




# Handle loading times with redux

`npm i redux-thunk`


*ADD loading parameter to balanceReducer objects*



*in main index.js*
```jsx

//after all other imports

import thunk from 'redux-thunk'

//middleWare required
import { createStore, combineReducers, applyMiddleware } from 'redux'

const store = createStore(combineReducers({
	balanceReducer, loanReducer
}), 
	applyMiddleware(thunk)) // line that changed





```


*Create new folder with actions*

*actions/balanceActions.js*

```jsx

//This action is the same as the one created with useDispatch()

export function deposit() {
	return {type: "DEPOSIT", payload: 100}
}

export function depositAsync() {
	return dispatch => {
		//just to simulate delay
		dispatch(loading())
		setTimeout(()=>{
			dispatch(deposit())
		}, 2000)
	}
}


//function for loading

export function loading() {
	return {
		type:"LOADING"
	}
}



```


*implement actions in deposit*

```jsx
import React from 'react'
import {useSelector, useDispatch} from 'react-redux'
import * as balanceActions from '../actions/balanceActions'

const Deposit = ()=>{

	const balance = useSelector(state =>{
		return state.balanceReducer.balance
	})


	const loading = useSelector( state => state.balanceReducer.loading)

	const dispatch = useDispatch()


	//new bits
	const handleDeposit = () =>{

		dispatch(balanceActions.depositAsync())

	}


}



```



## Add conditions to main div where the button is

`{loading ? <h3> Something is happening </h3> : <button>Button for the thing</button>}`















# Nice css for navbar
```css
ul {
  margin: 0 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 0;
}


a {
  color: #222;
  font-size: 22px;
}

.active {
  color: red;
}

ul {
  list-style: none;
}
```