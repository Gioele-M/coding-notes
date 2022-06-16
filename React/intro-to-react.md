Introduction
===============
## WTH is...
## React?
A javascript library which renders components to create sites.

### JSX?
A language extension allowing you to write html elements directly in your .js files, as you would otherwise when inserting content with .innerHTML
e.g. 
```
ReactDOM.render(
	<h1>Hello World</h1>
	document.getElementById('root')
	)
```
- Variables can be interpolated into JSX with {curly bracers}
<p align="right">(<a href="#top">back to top</a>)</p>

### State?
A built in object used to store information
<p align="right">(<a href="#top">back to top</a>)</p>

### Hook?
Allows you to use state and other react features _without_ writing a class

Hooks are functions that let us "hook" into the React state and lifecycle features from function components.

- hooks don't work in classes
- only call them at the top level of code

add listeners directly to elements

Two methods of changing a value in state
```js
setCounter(counter + 1)
setCounter(prevCounter => prevCounter + 1)
```
<p align="right">(<a href="#top">back to top</a>)</p>

#### Glossary
CRA = Create React App using the create-react-app package
Non-CRA = Initializing a React App manually without the 'create-react-app' command
SPA = Single Page Application
JSX = JavaScript Syntax Extension / JavaScript XML

<p align="right">(<a href="#top">back to top</a>)</p>
