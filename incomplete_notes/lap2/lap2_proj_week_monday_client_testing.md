# Client side testing

Use 'npm i -D live-server' for automatic server refreshes


## Testing setup
You need 
- `npm i -D jest` 
- `npm i -D jest-environment-jsdom` 

*In test folder - layout.test.js*
```js

/**
 * @jest-environment jsdom
 */

const fs = require('fs')
const path = require('path')
const html = fs.readFileSync(path.resolve(__dirname, '../index.html'), 'utf8')
// This creates a string, not a DOM, so dynamic changes are not accounted for




describe('index.html', ()=>{
	beforeEach(()=>{
		document.documentElement.innerHTML = html.toString()
	})

	test('it has title', ()=>{
		let header = document.querySelector('header')
		const title = document.querySelector('title')
		expect(title).toBeTruthy()
	})

	it('h1 is empty when website loads', ()=>{
		const h1 = document.querySelector('h1')
		expect(h1.innerHTML).toContain('')
	})


	//this wont work because we have a string not a dom 
	it('displays "morning" when button is clicked', ()=>{
		const btn = document.querySelector('button')
		//Click button and check content after
		btn.dispatchEvent(new Event('click'))

		const h1 = document.querySelector('h1')
		expect(h1.innerHTML).toContain('morning')

	})

})


```



In *helpers.js* -> builds dom object for dynamic testing
```js
const path = require('path')
const jsdom = require('jsdom')
const {JSDOM} = jsdom


const renderDOM = async (filename)=>{
	//find file path
	const filePath = path.join(process.cwd(), filename)

	const dom = await JSDOM.fromFile(filePath, {
		runScripts: 'dangerously',
		resources: 'usable'
	})

	//Return promise that loads the dom only after everything else is loaded
	return new Promise((resolve, reject) =>{
		dom.window.document.addEventListener('DOMContentLoaded', ()=>{
			resolve(dom)
		})
	})
}

module.exports = renderDOM

```



*Back in layout.jest.js*
```js
// Add new require for the renderDOM fn
const renderDom = require('./helpers')

//before describe
let dom
let document


//inside describe, replace beforeEach()
beforeEach(async ()=>{
	dom = await renderDOM('index.html')
	document = await dom.window.document
})


//now change the test that was previously not working
	
it('displays "morning" when button is clicked', ()=>{
	const btn = document.querySelector('button')
	//Click button and check content after

	//have the DOM create the new Event
	btn.dispatchEvent(new dom.window.Event('click'))

	const h1 = document.querySelector('h1')
	expect(h1.innerHTML).toContain('morning')

})


//other example
it('adds the input value to the h1', ()=>{
	const form = document.querySelector('form')
	const h1 = document.querySelector('h1')

	const input = document.querySelector('#name')
	input.value = 'baby yoda'

	//Send event
	form.dispatchEvent(new dom.window.Event('submit'))

	expect(h1.innerHTML).toBe(input.value)

})


```























