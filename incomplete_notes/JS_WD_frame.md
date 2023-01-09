# JS WebDevelopment Frame
## Includes bundling, server and testing

### Standard files layout
.
-index.html

-static/
	-image/
		-favicon.png
	-js/
		-index.js
		-other.js
	-css/
		-style.css
	
-test/
	-layout.spec.js

-coverage/




*Initialize npm*
`npm init -y`

# Packages


### For development
- watchify -> bundler that fixes dependencies automatically
- concurrently -> run commands together 
- jest
- jest-environment-jsdom -> acts like your dom 


### For APIs
- npm install i -D jest nodemon supertest (Need nodemon this so to not restart the server to implement pages)




## In npm environment (.json)
```
"dev": "concurrently \"watchify ./index.js -o bundle.js\" \"python -m http.server\""
"test": "jest --watchAll"
"coverage": "jest --coverage"
```


## Standard header for test file layout.spec.js
```js
/**
* @jest-environment jsdom
*/

const fs = require('fs') -> to interact w files
const path = require('path') -> to determine where files are at

//Build html
const html = fs.readFileSync(path.resolve(_dirname, '..index.html'), 'utf8');   //(one or two underscores before)

// YOU ALSO NEED TO SET THE DOCUMENT TO VALUE OF HTML STRING INSIDE EACH 'DESCRIBE' DECLARATION
beforeEach(() => {
        document.documentElement.innerHTML = html.toString();
    })
```


---

# For APIs
- need Express no dev
	+ npm install express (no -D)


































### Be careful!
- Remember to link 'bundle.js' to HTML
- Remember to add beforeEach() for the html.toString()
- Remember to add afterEach() when running an API 











