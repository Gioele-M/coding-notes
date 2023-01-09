# Intro to React

- React is a library _not_ a framework
- Useful for DOM interaction
- Need at least version 16, if you have lower than 14 probably things will break

## Create react app
`npx create-react-app <appName>`


### For this example

- `npx create-react-app monday`

- `cd monday` -> enter in the app folder
	+ This enters a git-initialised file where the app layout is stored

- Have a look to README to see how to proceed with scripts

- React is a SPE -> single page applications -> single HTML file

- You get a template to start with
	+ Modify App.js to change HTML
	+ index.js to modify HTML interactions
		* `root.render(<React.StricMode> <App /> </React.stricMode>)`
			- Used to check errors in app, loads App elements (class constructor which has HTML)
	+ index.css for CSS obv


- In settings.json
	+ For shortcuts
		* `"emmet.includeLanguages": {
		    "erb": "html", //maybe not really need this
		    "javascript": "javascriptreact",
		    "typescript": "typescriptreact"
		  }`


- Now that everything is set 'build' app with 
	+ `npm run eject`
	+ This cannot be reversed





# Build react app from scratch

- `npm init -y`
- `npm i react@17 react-dom@17` //@n is for version number
- `mkdir src public config`
- need .babelrc file

- In src file
	+ create `index.js` 
	+ Extension can also be JSX (JS-XML) (XML same as HTML but no predefined tags ex h1)
	+ in BABELJS.IO you can see how the code is converted
	
- In public
	+ `index.html`
	
- In config
	+ make webpack.config.(|dev|production).js
	+ get config from this repo
		* https://github.com/getfutureproof/fp_study_notes_intro_to_react/tree/master/config



*index.js*
```jsx

import React from 'react'
import ReactDOM from 'react-dom'

ReactDOM.render(<h1>Hello world</h1>, document.getElementById('root')) //This would render a plain Hello world in our page 

```


*index.html*
```HTML
Standard template + div with 'root' id
That div gets targeted in index.js


```


*package.json*
```json

"scripts": {
    "dev": "webpack-cli serve --mode development --config config/webpack.config.dev.js",
    "build": "webpack --config config/webpack.config.production.js"
  },
  "devDependencies": {
    "@babel/plugin-proposal-class-properties": "^7.12.1",
    "babel-loader": "^8.2.1",
    "css-loader": "^5.0.1",
    "html-webpack-plugin": "^4.5.0",
    "style-loader": "^2.0.0",
    "webpack": "^5.5.1",
    "webpack-cli": "^4.2.0",
    "webpack-dev-server": "^3.11.2"
  },
  "dependencies": {
    "@babel/core": "^7.12.3",
    "@babel/preset-env": "^7.12.1",
    "@babel/preset-react": "^7.12.5",
    "react": "^17.0.1",
    "react-dom": "^17.0.1"
  }

//Need to npm i after you add all this stuff

```


*All config files are in repo linked above*


*.babelrc*
```
{
	"presets": [
	"@babel/preset-env", 
	"@babel/preset-react"
	],
	"plugins": [
		[
		"@babel/plugin-proposal-class-properties"
		]
	]
}
```


### Create first component

*in index.js*

```jsx
const App = ()=>{
	return(
		<h1>Hello world</h1>
	)
}

//OR DIFFERENT SYNTAX

const App = () => <h1>Hello world</h1>

// Add more than one element

const App = ()=>{
	return(
		<React.Fragment> 
		<h1> Hello there </h1>
		<h2> hello 2 </h2>
		</React.Fragment>
	)
}


//Now you can add it to the reactDOM render

ReactDOM.render(
	<App />, 
	document.getElementById('root')
	)

```

*In src file, create App.js and move App component declaration there*
```jsx

const App = ()=>{
	return(
		<React.Fragment> 
		<h1> Hello there </h1>
		<h2> hello 2 </h2>
		</React.Fragment> // can be substituted with empty <>
	)
}

export default App

// Now you need to import in index.js

import App from './App'

```


## In src file

- Create components file
	+ in here add all components required (remember to import in index.js)

*Show variables in components*
```jsx

import React from 'react'

const TrainerCard = ()=>{
	let pokemon = 0
	return(
	<div>Pokemon number: {pokemon}</div>
	)
}

export default TrainerCard
```

*Count variables in components*
```jsx

import React from 'react'

const TrainerCard = ()=>{
	const [count, setCount] = useState(1) //Will give value 1 to those variables - not really used properly here, will do later

	const pokemons = ['x', 'y', 'z']

	const renderLis = () => {
		pokemons.map(p => <li>{p}</li>)
	}

	return(
		<>
		<div>Pokemon batch: {count}</div>
		<ul className='class'> //Use className instead of class (check console logs)
			{renderLis()}
		</ul>
		</>
	)
}

export default TrainerCard
```



