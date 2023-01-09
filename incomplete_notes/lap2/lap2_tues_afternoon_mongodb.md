# MongoDB
NoSQL 
MongoAtlas -> get database without config (less work than heroku and postgres)


## Class demo

In new repo 

Docker terminal

docker run --name first-part -d mongo -> make container with mongo image

docker ps --no-trunc -> see container id

docker exec -it first-part mongo (mongosh easier (?))


mongo -> opens mongo terminal 

- show dbs 
	+ Shows databases
	+ show collections
		* shows collections
	
- {object}
	+ shows object name 

- db.stats()
	+ shows stats about db
	+ db.stats().{parameter} -> will get parameter from stats

- use {dbName}
	+ switches to db dbName
	+ IF DB doesn't exist it creates one

- db.createCollection('{nameInCamel}')
	+ creates collection in db



*Create*
- db.{collectionName}.insertOne({object}) ({name: "Zelda", age:3, "owner": "Aki"})
	+ appends data to table
		* gives inherently an objectID (with sort of timestamp in exadecimal)
	+ Makes table if not present

- db.{collectionName}.insertMany([{objects},{array}]) 


*Find*
- db.{collectionName}.find()
	+ shows collection content
	+ Chain other functions eg
		* db.{collectionName}.find().count()

- db.{collectionName}.find({query_options_object})
	+ ex.
		* db.cats.find({name: {$eq: "Zelda"}})
			- Looking for a cat named Zelda
			- ($ -> indicates the calling of a function, in this case equals)
	+ ex2.
		* db.cats.find({owner: { $exists: true }})
			- find cats which have the owner key
				+ $exists checks for existance
	+ ex3.
		* `db.cats.find({owner: { $exists: true }}, {name:true, _id:0 })`
			- show only the name not full object
			- `_id:0` removes id keys from returns



- let allCats = db.cats.find().toArray()
	+ make a variable instance with the content of cats and cast it into array
	+ !! DOUBLE CHECK IF THIS INSTANCE REFRESHES AUTOMATICALLY OR NOT WHEN OBJECT IS ADDED



*Update*
- db.{collectionName}.updateOne({attribute:"value"}, {$set: {attribute:value}}) 
	+ ex. ({name:"Zelda", {$set: {age: 4}}})
	+ Update instance parameter
		* $set

- db.{collectionName}.updateMany({attribute:{$gt: value}}, {$set: {attribute:value}})
	+ updates all objects with attribute $gt (greater) than the set value to new parameter
	+ $gt greater than
	+ first obj is the logical selector for what you want to update, second is the new key/pair


*Delete*
- db.{collectionName}.deleteOne({attribute: "value"})
	+ deletes instance with attribute of value value

- db.{collectionName}.deleteMany({attribute: {$gt: value}}) -> filled same as before, with logical condition
	+ $gt but could be any condition

- db.cats.deleteMany({})
	+ deletes all

- db.{dbName}.drop()
	+ remove database
- db.dropDatabase()
	+ removes current database(?)




## Class example
CONNECT FILES IN MY MACHINE TO THE CONTAINER

---
mkdir demo; cd code

touch {shelterHelpers,seedDogs}.js
code .


---

### In shelterHelpers.js
```js
let db = connect("localhost:27017/{dbName}") //db= shelter // THAT LOCALHOST is default for mongo

function getAdultCats(){
	return db.cats.find({age:{$gt: 3}})
}

function findDogByBreed(queryBreed){
	return db.dogs.find({ breed: {$eq: queryBreed}})
}

```

### In seedDogs.js
```js
let db = connect("localhost:27017/shelter")
//insert dogs
db.dogs.insertMany([
  { name: 'Mochi', breed: 'shi-pu', ownerName: 'Naz' },
  { name: 'Masha', breed: 'shih-tzu', ownerName: 'Vesna' },
  { name: 'Hendon', breed: 'golden retriever', ownerName: 'Vesna' },
  { name: 'Zola', breed: 'golden retriever', ownerName: 'Beth' },
  { name: 'Snip', breed: 'greyhound' }
])

```

---
*back in docker terminal*
We create a container named shelter-db and put our data in 
```bash
docker run --name shelter-db -d --mount type=bind,source="${pwd}",dst="/code" mongosh

#Now you're in mongo

> ls()  		#shows all folders in container

> pwd() 		#shows current dir

> cd("directory") #changes to this directory

> show dbs 		#shows all databases
> show collections #shows all collections

> load("filename.js") #loads js file and requires all functions/variable

> db.stats() 	#shows database info

> # Now you can use the functions saved in the js file to load db or interact with it



# Commands examples

> #Order cats alphabetically -> cats is {collectionName}
> db.cats.find().sort({name:1}) # 1 equals true #0 false #-1 inverse
	>Sort by age in descending order
	> .sort({age: -1})

> #Find with regular expressions
> db.cats.find({"name": /^Z/})
	>find cats whos name start with z 
	> .limit(1)
		>return only first instance


```



### Create container sample
```bash
docker run -d \ 
	--name shelter-db \
	--mount type=bind,source="${pwd}",dst="/code" \
	mongo
```
#-d run on background
#so you need script to initialise, best if with mongosh

```bash
docker exec -it shelter-db mongosh

```


# Collection type object validators

```js

//Simple validator

db.createCollection("dogs", {
	validator: { $and:[
		{name: {$type: "string"}},
		{age: {$type: "int"}}
		]ì
		
	}
})


// incomplete complex validator
db.createCollection("food_supplies", {
	validator:{
		$jsonSchema:{
			bsonType: "object",
			required: ['array of required parameters']
			properties:{
				name: {
					bsonType: "string",
					description: "must be string"
				},
				address:{
					bsonType: "object",
					required: [...]
				}
			} ...
		}
	}
})



```




# Check documentation for mongo $functions




