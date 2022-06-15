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

