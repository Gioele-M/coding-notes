# Testing

CRA - create react app
SPA - single page application
RTL - react testing library



### Test components and its elements

Need testing library RTL (react testing library)

- Without CRA initialisation
`npm i -D @testing-library/react`
Many more imports, have a look at the wiki, best if you start with CRA

- With CRA you don't really need it as already present

*This still requires JEST as RTL only adapts the library, doesn't provide a frameworj for testing*

---

#### Setup for testing

*In package.json*

```json
{
	"other":"stuff",

	"scripts":{
		"test": "react-scripts test --setupFilesAfterEnv ./src/test/setupTests.js",
		"start": "PORT=4000 react-scripts start" //this overrides behaviour and starts server in port 4000 instead of 3000
	},

	"jest": {
    "moduleNameMapper": {
      "\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga)$": "<rootDir>/__mocks__/fileMock.js",
      "\\.(css|less)$": "<rootDir>/__mocks__/styleMock.js"
    }
  }
}
```


*__Mocks__*
- Create a new folder in top level called `__mocks__`
	+ in it make file `fileMock.js` with content `module.exports="test-file-stub"`
	+ also make file `styleMock.js` with content `module.exports={}`

*In src*

- Make file test
	+ make file `setupTests.js`

*setupTests.js*
```js
import React from 'react'
import {render} form '@testing-library/react'
import userEvert from '@testing-library/user-event'
import '@testing-library/jest-dom/extend-expect';
 

//These are required so not to be imported in every file
global.React = React
global.render = render
global.userEvent = userEvent


```


---


#### Testing environment file structure

```
-/myApp
    -/__mocks__
        -fileMock.js
        -styleMock.js
    -/node_modules
    -/public
    -/src
        -/test
            -App.test.js
            -setupTests.js
        -App.js
        -index.js
    -package.json
    -package-lock.json

```


### Test counter component (reactStates.md first example)
Make file Counter.test.js

*in Counter.test.js*
```js
import { screen } from '@testing-library/react'
//Screen is basically the document

import Counter from '../Counter'

describe('Counter', ()=>{
	const setUp()=>{ render(<Counter />) }

	beforeEach(()=>{
		setUp()
	})

	test('it loads with the counter at 0', ()=>{
		const counter = screen.getByRole('counterNumber') //You need to add role attribute in component elements to be able to target them
		expect(counter.textContent).toBe(0)
	})

	test('clicking the btn increases the counter by one', ()=>{
		const counter = screen.getByRole('counterNumber')
		const button = screen.getByRole('button', {name: "add one"}) // all buttons have by default role button, the name is determined by the text content, in this case add one

		//This should work
		//userEvent.click(button)

		//Use fire event instead (need to import from @testing-library/react)
		// import {fireEvent} from '@testing-library/react'
		//you import it with render in setupTests.js, add there and make global
		fireEvent.click(button)

		expect(counter.textContent).toBe("1")


	})

})

```


*Example give component a role*
```jsx

return(
		<div>
			<div role="counterNumber">Count: {counter} </div>

			<button onClick={()=> handleBtnClick()}>
				add one
			</button>
		</div>

	)

```


*screen options*
https://testing-library.com/docs/queries/about
https://testing-library.com/docs/queries/about#types-of-queries
- .getBy - .getAllBy
- .queryBy - .queryAllBy
- .findBy - .findAllBy 



*Then*
`npm run test`















