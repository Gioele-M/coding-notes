# Restructuring apps


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



