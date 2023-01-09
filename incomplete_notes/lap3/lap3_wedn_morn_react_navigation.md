# Navigation

### React router
`npm i react-router-dom@6`
Look at the documentation


## Work with different pages with BrowserRouter

*Main index.js usage*
```jsx
import {BrowserRouter} from 'react-router-dom'


ReactDOM.render(
	<React.StrictMode>
		<BrowserRouter>
			<App />
		</BrowserRouter>
	</React.StrictMode>, 
	document.getElementById('root')
)

```



## Context.provider (?)
Provides a way to pass data through the components without using props




# Routes and route

*In App.js*

```jsx
//All regular imports

import { Routes, Route } from 'react-router-dom'

function App(){

	return(
		<>
			<Header /> // This is the component that stores the navbar for navigation

			<Routes>

				<Route path="/" element={<News />} />
				<Route path="about" element={<About />} />
				<Route path="home" element={<Home />} />

				//for the 404
				<Route path="*" element={<NotFound />} /> 

			</Routes>

			<Footer />
		</>

	)
}


```

Router and routes allow for page navigation - works with pages
- Path -> checks path after url
- Element -> gives element to render




# Link for navbar

(*Header/index.js*)
```jsx
import React from 'react'
import { Link } from 'react-router-dom'
import './style.css'

const Header = ()=>{
	return(

		<ul>
			<li><Link to="/">News<a/></li>
			<li><Link to="/about">About<a/></li>
			<li><Link to="/home">Home<a/></li>
		</ul>

	)
}


```



# Navlink better than Link
Navlink alows you to check for active state

(*Header/index.js*)
```jsx
import React from 'react'
import { navLink } from 'react-router-dom'
import './style.css'


const Header = ()=>{
	//Function to handle active link
	const activeClass = ({ isActive }) => ( isActive ? 'current' : undefined )

	return(

		<ul>
			<li><navLink className={ activeClass } to="/">News<navLink/></li>
			<li><navLink className={ activeClass } to="/about">About<navLink/></li>
			<li><navLink className={ activeClass } to="/home">Home<navLink/></li>


			//Button to go back in navigation
			<BackButton />
		</ul>

	)
}


// This is the logic for the className
// Extracts isActive boolean from navLink class, checks if active, if it is it changes the style
//style = {({isActive}) => isActive ? activeStyle : undefined}

```



# Button to go back

*in components/BackButton/index.js*

```jsx
import React from 'react'
import { useNavigate } from 'react-router-dom'


const BackButton = () => {
	const navigate = useNavigate


	return (
		<button onClick={()=>{navigate(-1)}}>
			Go back
		</button>



	)
}


```



# To add a 404

```jsx
import React from 'react'
import { useLocation, Link } from 'react-router-dom'


const NotFound = ()=>{
	const location = useLocation()

	return(
		<>
			//Let user know that his location is not available
			<h1>
				Sorry, {location.pathname} not found
			</h1>
			//offer link to main page
			<Link to="/" >
				Go back to main page
			</link>
		</>

	)
}

export default NotFound

```


## Test for back button
Requires simulation of environment as when we test the component, it is on its own. Need MemoryRouter
Need the files for setting tests up and __mocks__ folder


```jsx
//This would be the document
import { screen } from '@testing-library/react'

import { MemoryRouter } from 'react-router-dom'
import BackButton from './'

describe('back button', ()=>{
	beforeEach(()=>{
		render(<BackButton />, { wrapper: MemoryRouter})
		//puts backbutton in memory router
	})

	test('renders a back button', ()=>{
		const btn = screen.getByRole('button', {name: 'go back'})
		expect(btn).toBeInTheDocument()
	})

})

```

