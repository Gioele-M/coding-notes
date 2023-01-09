# Object Oriented Programming (OOP)

### Object in JS

```js
const objectName = {
	property1 = 'xxx'
	property2 = 'yyy'
	property3 = null
	methodName: function(){
		this.property1 ? return true : return false
	} // !!! DO NOT USE ARROW FUNCTIONS BECAUSE THIS KEYWORD WONT WORK
}
```
*THIS* makes it refer to the current object

Access properties and methods with '.'
- objectName.property1
- objectName.methodName()


## Factory functions

function Animal(name, dob, owner=null){
	this.name = name
	this.dob = dob
	this.owner = owner

}


`
//THIS WAS USED IN ES5
{
*_Add function with prototype_
Animal.prototype.speak = function(){
	console.log(...)
}
*_Add property_
Object.defineProperty(Animal.prototype, 'propertyName', { //Function to define what goes inside
	get(){
		return this.owner
			? 'Adopted'
			: 'Not adopted'
	}
})
}
`

### In ES6 instead you can declare methods inside the body of the object

`js
{
	adopt(newOwner){
		....
	}
}`



## Inherit class
cat is inheriting from animal


---
### Inheritance in ES5
```js
function Cat(name, dob, owner){
	Animal.call(this, name, dob, owner)
}

//Need to let JS know the objects are linked
Object.setPrototypeOf(Cat.prototype, Animal.prototype)

//Still need to add functions like this for 'children' objects
Cat.prototype.speak = function(){
	console.log('...')
}

// Now you can creat a cat object
let catto = new Cat('Catto', 101010)
```

---

## Inheritance in ES6

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



