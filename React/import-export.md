📫 Importing/Exporting
======================
React being a series of modular components, there is a lot of importing/exporting going on!

## Key Points
- **Import** always defaults to looking for **index.js** (at least in React)
*e.g.* `import BackButton from '.'` *- this would look for index.js in the current folder*
- Remember that you must **export** every component!

### Consolidating Export Paths
- To make life easier, in the root of your component folder, in index.js, it is helpful to include an additional export line for each component:
`e.g. export { deafult as Greeting } from './Greeting'`
- You can now **import** from the components root folder itself rather than each individual component file!

## Notes on Import Vs. Require
- import() is ES6 syntax
- require() is CommonJS, needed before ES6
- require() is needed for plugins
- import() *cannot* be called conditionally, must be at beginning of the file
- import() can read .mjs files
- ES Modules can be loaded dynamically via import()

## Import

> Sources: 
> [Stack Post](https://stackoverflow.com/questions/36795819/when-should-i-use-curly-braces-for-es6-import)
> [Import - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)

### Default Import
This is a default import:
```js
// B.js
import A from './A'
```
It only works if A has the default export:
```js
// A.js
export default 42
```
With default, it doesn’t matter what name you use for assignment when importing:
```js
// B.js
import A from './A'
import TheresSomethingAboutAlfred from './A'
```
It will always resolve to whatever is the **default** export of A.

#### Renaming
Default export is also a named export with the name 'default', so it can also be imported and then renamed like this:
```js
import {default as Sample} from '../Sample.js';
```
*Inside the curly brackets here we are returning the default property and renaming it as sample*

### import *
Import everything and assign it to an object.
```
import * as Mix from "./someplace.js"
```

### Named Import & Curly Brackets
When importing/exporting multiple named items, they must be encapsulated in `{ curly, brackets }`

The brackets are also used when pulling specific things from an export, just like specific properties in an object.

```
import { add } from "./math.js"
```

## Export
> Sources:
> [Export - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

### Named Exports
**Named exports** are useful to export several values. During the import, it is mandatory to import them within curly braces with the same name of the corresponding object. 
```js
// export features declared earlier
export { myFunction, myVariable };
```

You can also rename named exports to avoid conflicts:
```js
export { myFunction as function1,
         myVariable as variable };
```
### Default Exports
- a **default** export can be imported with *any* name
- a module can **only** have one default export

```js
export { myFunction as default };
```

### Chaining Exports
An export can be joined to another export.
```js
// .src/components/Greeting/index.js
export const Greeting = () => {/* code */}

//	./src/index.js
export { Greeting } from './Greeting';

// ./pages/News.js
import { Greeting, ReaderCount, Headlines, FeaturedArticle } from '../components';
```

If exporting default from the component, modify the line in ./src/index.js
```js
export { default as Greeting } from './Greeting'
```

#### export * 
This can only be used when re-exporting, used when exporting multiple named exports. (I think)
```js
export * as Greeting from '/Greeting'
```
