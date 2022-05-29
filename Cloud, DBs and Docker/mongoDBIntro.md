# MongoDB and NoSQL

---

# NoSQL
*NoSQL (Not Only SQL)*
MongoAtlas -> get database without config (less work than heroku and postgres)

### NoSQL implementations
- Key-value pair:
	+ Type of database where all relevant information is stored underneath a key. Example is the user_id-data where all user's data is collected under its id

- Document store:
	+ Similar to key-value pair, but less standardised in what is stored into the value. Example would be a collection of users all with same core parameters but some might be added/missing (ex not all have 2nd IP etc)	

- Graph databases:
	+ Graphs are created from collections of nodes and the connections between them. This means that we need to store info about a specific thing, then also create connections between these documents and store info about these connections.


---


# MongoDB
NoSQL database framework - MongoDB compass is the GUI


## Docker usage
- Run MongoDB in a new container
	+ `docker run --name <container-name> -d mongo`
	+ This opens it in detached mode (-d flag)

- Create container with volume
	+ `docker run --name <container-name> --mount type=bind,source="(pwd)",dst=<destination-in-user-machine> -d mongo`

- Attach container in bash shell
	+ `docker exec -it <container-name> /bin/bash`
- Attach to container in mongo shell
	+ `docker exec -it <container-name> mongo` -> can also do mongosh
	+ `exit` to leave
	
- See current container id
	+ `docker ps --no-trunc`


### MongoDB terminal commands
All of this can be done in a JS file and opened in the mongo terminal with `load('filename.js')`

- Use this database. If it doesn't exist it makes one
	+ `use {dbName}`


*Create*

- Create collection in DB
	+ `db.createCollection('{nameInCamel}')` -> can add restrictions but not necessary

- Append data to collection
	+ `db.{collectionName}.insertOne({object})`
		* object example -> {name: "Zelda", age:3}
	+ This gives inherently an objectID composed of timestamp in exadecimal
	+ Makes the collection if not present

- Append multiple elements to collection
	+ `db.{collectionName}.insertMany([{obj},{array}])`

---

*Read*

- See all databases
	+ `show dbs`

- See all collections
	+ `show collections`

- Show object name
	+ {object}

- Show current database stats
	+ `db.stats()`
	+ Get specific stats parameter
		* `db.stats().{parameter}`

- Shows collection content
	+ `db.{collectionName}.find()`
	+ Can chain other functions such as .count() or .limit()

- Use find with function
	+ `db.{collectionName}.find({query_option_object})`
	+ ex
		* `db.cats.find({name: {$eq: "Zelda"}})`
		* Find cat named Zelda
	+ ex2
		* `db.cats.find({owner: { $exists: true }})`
		* Find cats with owner key
	+ ex3
		* `db.cats.find({owner: { $exists: true }}, {name:true, _id:0 })`
			- show only the name not full object
			- `_id:0` removes id keys from returns
			
- Make a variable instance with the content of cats and cast it into array
	+ `let allCats = db.cats.find().toArray()`

- Perform action for each returned element in cursor
	+ `db.cats.find().forEach(cat => print(cat.name))`

---

*Update*

- Update one parameter
	+ `db.{collectionName}.updateOne({attribute:"value"}, {$set:{attribute:"value"}})`
		* ex. ({name:"Zelda", {$set: {age: 4}}})

- Update many
	+ `db.{collectionName}.updateMany({attribute:{$gt: value}}, {$set: {attribute:value}})`
		* This updates all objects with attribute $gt than the given value
		* First object is the logical selector for what you want to update and the second is the new key/pair

---

*Delete*

- Delete instance in collection
	+ `db.{collectionName}.deleteOne({attribute:"value"})`

- Delete many instances
	+ `db.{collectionName}.deleteMany({logical condition object as before})`

- Delete all in collection
	+ `db.{collectionName}.deleteMany({})`

- Remove collection
	+ `db.{collectionName}.drop()`

- Remove current database
	+ `db.dropDatabase()`


### Extra operations

_Sorting_
Requires to know what we're sorting on and which direction (1 ascending, -1 descending)

ex. Sort collection in ascending order by age
`db.{collectionName}.find().sort({"age": 1})`

ex. Return youngest cat with name beginning with Z
`db.cats.find({"name": /^Z/i}).sort({"age":1}).limit(1)`

---

_Aggregation_
Aggregation is done in pipeline of actions that include matching, grouping and more. Examples

- Calculate total price of all items
	+ `db.list.aggregate({$group: { _id: '', total: { $sum: '$price'} }})`
		* $price is the column declaration

- Calculate total price 
	+ `db.list.aggregate({$group: { _id: '$category', total: { $sum: '$price'} }})`
	
- Get only items that cost more than 2, then calculate price of each category
	+ `db.list.aggregate([
    {$match: { price: { $gt: 2} }},
    {$group: { _id: '$category', total: { $sum: '$price'} }}
    ])`

---

_Joins_
Although non-relational, $lookup allows performing a join

- Get full join result
`db.owners.aggregate([
    { $lookup:
        {
           from: "dogs",
           localField: "pet",
           foreignField: "name",
           as: "petDetails"
        }
    }
])`

- Add a match to the pipeline to filter your results
`db.owners.aggregate([
   { $match: { name: 'Beth'} },
   { 
        $lookup: {
             from: 'dogs',
             as: 'pets',
             let: { owner: '$name' },
             pipeline: [{ $match: { $expr: { $eq: ['$ownerName', '$$owner'] } } }]
        }
    },
]).pretty()`

---

## mongoDB operators
https://www.mongodb.com/docs/v5.0/reference/operator/query/

##### Comparison
- `$eq`
	+ matches values equal to a specified value

- `$gt`
	+ matches values greater than 

- `$gte`
	+ matches values greater than or equal

- `$lt`
	+ matches values less than 

- `$lte`
	+ matches values less than or equal

- `$in`
	+ matches any value specified in an array

- `$ne`
	+ matches all values that are not equal to the specified one

- `$nin`
	+ matches none of the values specified in the array


##### Logical

- `$and`
	+ joins query clauses with logical AND

- `$not`
	+ inverts the effects of a query expression

- `nor`
	+ joins query clauses with logical NOR (returns all documents that fail to match both clauses)

- `$or`
	+ Joins query clauses with logical OR


##### Element

- `$exists`
	+ matches documents that have the specified field

- `$type`
	+ matches documents if a field is of the specified type


##### Evaluation

- `$expr`
	+ Allows use of aggregation expressions within the query language

- `$jsonSchema`
	+ Validate documents against the given JSON Schema

- `$mod`
	+ Performs a modulo operation on the value of a field and selects documents with a specified result

- `$regex` 
	+ Selects documents where values match a specified regular expression
	
- `$text`
	+ Performs text search.

- `$where`
	+ Matches documents that satisfy a JavaScript expression.


##### Array

- `$all`
	+ Matches arrays that contain all element specified in the query

- `$elemMatch`
	+ Selects documents if element in the array field matches all the specified conditions
	
- `$size`
	+ Selects documents if the array field is a specified size


##### Projection and micellaneous

- `$`
	+ Projects the first elemetn in an array that matches the query condition
	
- `$meta`
	+ Projects the document's score assigned with $text operation
	
- `$slice`
	+ Limits the number of elements projected from an array

- `$comment`
	+ Adds a comment to a query 

- `$rand`
	+ Generates a random float between 0 and 1

- `$set`
	+ To update instance of parameter
	
- `$sum`
	+ Sum vector

##### Geospatial and bitwise are not reported


---


# Schema and validation
Although NoSQL's strength is flexibility, we might want to set some fields to be validated before being stored. Examples of validation

### Simple validator
`db.createCollection("dogs", {
	validator: { $and:[
		{name: {$type: "string"}},
		{age: {$type: "int"}}
		]	
	}
})
`

### Validator with $jsonSchema
`db.createCollection("food_supplies", {
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
`

*Practical example*
`db.createCollection("food_supplies", {
    validator: {
       $jsonSchema: {
            bsonType: "object",
            required: [ "name", "price", "inventory", "supplier" ],
            properties: {
                name: {
                    bsonType: "string",
                    description: "must be a string and is required"
                },
                price: {
                    bsonType: "number",
                    minimum: 0,
                    description: "must be a neutral or positive value and is required"
                },
                inventory: {
                    bsonType: "number",
                    minimum: 0,
                    description: "must be a neutral or positive value and is required"
                },
                supplier: {
                    enum: [ "FoodStuffs", "PetsRUs", "FeedUs", "F.O.O.D" ],
                    description: "must be a registered supplier and is required"
                }
            }
       }
    }
 })
`


---

## Class Demo

We create a container named shelter-db and put our data in from our local 'code' folder where a collection was created in JS files

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



















