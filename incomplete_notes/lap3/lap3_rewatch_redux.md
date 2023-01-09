# Redux

Used to share state between different components

### Reducers

Reducers have a state and action

Action
- Action type dictates switch clause
- Action payload dictates amounts/variables


State
- Default value as state = intialState, required to return in the default clause of the switch statement to not delete stuff 


## Export reducer

Re import it in 

- main index - together with
	+ import { Provider } from 'react-redux'
		* to pass information (props) to rest of the app
	+ import { createStore } from 'redux'
		* to store props
	+ const store = createStore(reducer)
	+ in root wrap BrowserRouter in Provider tags + store={store}
		* Makes store available to all the app

	
## In files that need to access reducer info

- import {useSelector} from 'react-redux'

- In file function
	+ const variable = useSelector(state => state.variable)


## In files that need to interact with the info

- import { useDispatch } from 'react-redux'

- In file function need to dispatch an action (object with type field)
	+ const dispatch = useDispatch()
	+ const handleDispatch = () => { dispatch({type: "type", payload:"payload"}) }


---


# Add >1 reducer

- Filenames and functions need to be called something different
	+ reducerOne - reducerTwo
	+ Import them as such

- File content doesn't change 













