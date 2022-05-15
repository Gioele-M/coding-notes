# *Javascript*

---

## Data variables
- var -> Function or global scope. Does not have to be initialized to a value
- let -> Block scoped. Does not have to be initialised to a value
- const -> Block scoped. Must be initialized to a value and cannot be reassigned
- no-declaration -> Similar to var goes to global scope

--- 

## Data types

### Primitives

- string -> `"Hello world"`
- number -> `1`
- boolean -> `true / false`
- null -> representing intentional absence of value
- undefined -> value assigned to newly declared variables
- symbol -> used to make unique identifiers
- bigint -> used to identify whole numbers larger than 2^53 -1 

### Complex

- array -> `['a', 'b', 'c', true, 123, null]` 
- object -> `{name: 'Zelda', age: 3, 'hair colour': 'blue'}`
- function -> Declaration function `function <functionName> (parameter) {return parameter*2}` (only function declaration which works with hoisting)
- Arrow notation function -> `x => x+3`
- Expression function `const <functionName> = function(){return xxx}`

---

## Operators

- `+ - / *` -> add, subtract divide multiply
- `**` -> exponentiation
- `%` -> modulo - returns remainder of division
- `=` -> assigns value to variable
- `==` -> equal - returns true if both sides are equal (tries to convert to same data type before comparing)
- `===` -> strictly equal - returns true if values are **exactly** equal 
- `!` -> logical *NOT* eg. !true === false
- `&&` -> logical 'and'
- `||` -> logical 'or'

---

# If / Ternary / Switch

## If statements
`if(condition is true/truthy){
	//run code	
}else if(condition){
	//run code if condition true
}else{
	//run code if not met any condition
}
`
## Ternary statement
`condition ? code if true : code if false
age < 18 ? 'child' : 'adult'`

## Switch statements
`switch(variable){
	case <condition>:
		//do code if variable == condition;
		break;
	case <condition>:
		//do code;
		break;
	default:
		//code if no conditions are met
		break:
	}`

---
# Loops

## While loop
`while(condition is true){
	//run code
	//add a condition modifier or infinite loop
}`

## Do loop
`do{
	//code to run
	//same as before add condition modifier
}while (condition)`


## For loop
`for (let index=0; index </> condition; index++{
	//perform this code till stops
}`

---

# Iterators

## for...in (objects) / for...of (array)
Structure is the same but for...in is for objects and for...of is for arrays

`for(const key in object){
	//do this looping through keys
}
//
for(const element in array){
	//do this 
}
`

## forEach
forEach is used on arrays to apply a callback function (can establish an arrow function in it)
`array.forEach(element => console.log(element))`


## map
map is similar to forEach but it returns the values in a new array
`newArray = array.forEach(element => element.toUppercase())`


## some
some runs a callback function on the array until finds the element meeting the condition (not sure if can return anything)
`array.some(element => element.endsWith('x'))`


## filter
filter is similar to some but puts in a new array all the elements meeting the condition
`array.filter(element => element.endsWith('x'))`


## reduce (very tricky)
Basically it sums up (even if not numbers) all values in the array starting from the second argument and returns the total. If only 1 argument is given, it starts from 1st element of array. Other description:
It allows to keep a 'running total' of kinds (does not have to be numerical) and will pass it as an additional argument to the callback function to be operated on and returned. The final time, that running total becomes the return value of our reduce.
`let result = array.reduce((runningTotal, nextValue) => runningTotal + nextValue)`

## And many more...

---

# Functions

### 3 Types:
- In-built -> come with the language
- Libraries -> reusable functions collections
- Custom

### Declare and invoke
`function functionName(parameter, defaultParameter = 'xxx'){ //parameters are optional
	//code
	return toReturn //if any
}
functionName(argument)
`

### Declaration types
- Declaration function
`function functionName(){...}`
- Arrow function
`let functionName = () => {...} //brackets omitted if 1-liner`
- Expression function
`let functionName = function (){...}`

---

# Object Oriented Programming (OOP)

## Object in JS

```js
const objectName = {
	property1 = 'xxx',
	property2 = 'yyy',
	property3 = null,
	methodName: function(){
		this.property1 ? return true : return false
	}, // !!! DO NOT USE ARROW FUNCTIONS BECAUSE THIS KEYWORD WONT WORK
	//or!!
	methodName(parameters){
		// code
	}
}
```
*THIS* makes it refer to the current object

Access properties and methods with '.'
- objectName.property1
- objectName.methodName()

Access properties with {}
- data.students or {students} when passed as single parameter(?)


## Factory functions -> functions that return objects

function Animal(name, dob, owner=null){
	this.name = name
	this.dob = dob
	this.owner = owner
}

## Inheritance in ES6 -> other types in lap1-week1-thursday-afternoon

//All you need to inherit attributes and classes
```js
class Cat extends Animal {}
```

//If you want to add stuff need to modify the constructor
```js
class Dog extends Animal{
	constructor(name, dob, owner, dogBreed){
		super(name, dob, owner) //Same as parent class
		this.breed = dogBreed
	}

	//In here you can also add methods 
	speak(){
		//method code 
	}
}
```
---


*Objects* are instances of themselves and their parent class
catto instanceOf Cat -> true
catto instanceOf Animal -> true






