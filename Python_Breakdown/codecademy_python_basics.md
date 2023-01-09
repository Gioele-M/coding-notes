# Basic

### Comments
`#`


### Arithmetic operations
```
+ Addition
- Subtraction
* Multiplication
/ Division
% Modulus
** Exponentiation

+= Shorthand for adding
```


### Variables
Made of letters, numbers adn underscore (`_`)
Assigned with `=`

#### Types

Character
Boolean
Integer
Float


### Strings
Concatenation: use `+`
Return line: use `\`

### Elif statement
```py
if x == 0:
	print('0')
elif x == 1:
	print('1')
else:
	print('none')
```

### Operators
`or` -> works as logical or
`and` -> works as logical and
`not` -> works as logical not
`==` -> equal operator
`!=` -> not equal operator
`< > <= =>` -> comparison operators for numbers and characters



### Lists
Lists are oredered cllections of items, declared with `[]`
Lists accept micellaneous items and start at index 0. Can use negative indices
`my_list = []`

Access lists items with index -> `my_list[0]`
Nth depth lists can be accessed chaining `[]` eg `my_list[0][0]`


##### List Methods

Add lists together -> +
Slice -> [x:y] eg [1:] selects all but first element [:-1] selects all but last one

Append to list -> .append()
Insert in list -> .insert('x', 2) (item, index)

Remove from list -> .remove('x') (x is value to be removed)
Remove from list and return item -> .pop(n) (n is the index of value, default -1)

Count and return number of matching items -> .count('x') (in ['x', 'xy', 'y'] returns 1) 
Length of list -> len(my_list)

Sort list for number and characters -> .sort() (directly modifies the list instance)
										.sorted() (returns sorted list)




### Loops

##### KeyWords

Interrupt loop -> `break`
Skip loop iteration -> 'continue'

##### List comprehension
`[EXPRESSION for ITEM in LIST if CONDITIONAL]`
Ex.
my_list = [x**2 for x in range(10) if x%2 == 0]



##### For loop
`for TemporaryVariable in LIST:
	print(TemporaryVariable)`

`for num in range(10):
	print(num)`


##### While loop

`while CONDITION:
	print('x')`

`i = 1
while i < 6:
	print(i)
	i += 1
`



### Functions

Definition:
```py
global_variable = 'Hi'

def my_function(parameters, defaultParameter = 1):
	print(parameters, global_variable)
	scope_variable = 'accessible only inside function'
	return 1, 2, 3

x, y, z = my_function('argument')
```


### Strings
Strings are immutable, each time we perform an operation we create a new one

- Can be declared with 'single' or "double" quotes
- Are interpreted as lists of characters so all lists splicing works on strings too ([])
- Can also be concatenated with +
- Can also get the number of characters by calling len()
- Escape characters with `\`

- CHECK if character is in word -> 'v' in 'video'


#### String methods

Formatting methods:
.lower()
.upper()
.title() -> capitalises every word

Splitting methods:
.split(x) -> splits strings based on delimiter (x default space) | returns list

Joining strings
.join(x) -> joins list of strings 'x' adding character as delimiter ex. ' '.join(['x', 'y', 'z'])

Clean strings
.strip() -> Removes all white space characters from beginning and end, can strip specific character too if passed

Replace characters
.replace(x, y) -> Replaces x with y in strings

Find character
.find(x) -> Finds and returns the index of the first instance of x in the string, if none -1

Format string
'{} {}'.format(x, y) -> Replaces {} with given variables, you could also:
'{x} {y}'.format(x=x, y=y)



### Modules
A module is a collection of Python declarations intended broadly to be used as a tool. Modules are also often referred to as “libraries” or “packages” — a package is really a directory that holds a collection of modules.

Syntax: 
`from module_name import object_name as chosen_alias`

WORKING WITH FLOATS 
from decimal import Decimal
cost = Decimal('0.10')


### Dictionaries
Dictionaries are unordered sets of key:value pairs, keys cannot be a mutable type such as lists

Syntax
my_dict= {'x': 'y', 'm': 'n', ...}

Add a new value OR overwrite current value using
my_dict[key] = value

Add multiple values using
.update() -> my_dict.update({'w': 'p', 'u':'t'})

Get value by key
my_dict['x']

Get value by key without throwing errors (default return None)
my_dict.get(x)

Check if key exists
if key in my_dict:

Delete a key
.pop(value, default) -> removes key from dictionary if you know the value, returns default otherwise

Get list of keys
list(my_dict) or my_dict.keys() (immutable)

Get values
my_dict.values()

Get keys and values as tuples
my_dict.items()


##### DICTIONARIES COMPREHENSION
new_dict = {key:value for key,value in zip(list1, list2)}




### Files

Open file and read content
```py
with open('filename.txt') as file:
	content = file.read()

print(content)

```

Read content methods:
.read() -> returns file in a single liner
.readlines() -> returns file by line in a list
.readline() -> returns one line at the time


Write a file
```py
with open('new_filename.txt', 'w') as file:
	file.write('content to write')

```
`w` is used as writing mode (default r for reading) -> This overwrites a file
`a` is used for writing but appends at the end of the file


##### Working with CSV files

Reading:
```py
import csv
 
list_of_email_addresses = []
with open('users.csv', newline='') as users_csv:
  user_reader = csv.DictReader(users_csv, delimiter=';') #default delimiter ','
  for row in user_reader:
    list_of_email_addresses.append(row['Email'])
```
newline is used to make sure the line breaks do not result as new lines
csv.DictReader returns a list of dictionaries for each row with key the column title


Writing:
```py
big_list = [{'name': 'Fredrick Stein', 'userid': 6712359021, 'is_admin': False}, {'name': 'Wiltmore Denis', 'userid': 2525942, 'is_admin': False}, {'name': 'Greely Plonk', 'userid': 15890235, 'is_admin': False}, {'name': 'Dendris Stulo', 'userid': 572189563, 'is_admin': True}] 
 
import csv
 
with open('output.csv', 'w') as output_csv:
  fields = ['name', 'userid', 'is_admin']
  output_writer = csv.DictWriter(output_csv, fieldnames=fields)
 
  output_writer.writeheader()
  for item in big_list:
    output_writer.writerow(item)


```


##### Working with JSON files

Transform a JSON file to object: 
```py
import json
 
with open('purchase_14781239.json') as purchase_json:
  purchase_data = json.load(purchase_json)
 
print(purchase_data['user'])
```



Write object to JSON file:
```py
import json
 
with open('output.json', 'w') as json_file:
  json.dump(turn_to_json, json_file)
```



### Classes

Declaration:

```py
class CoolClass:
	class_variable_1 = 'X'

	def class_method(self, variable):
		print(self.class_variable_1, variable)


	#pass


```

Instantiation:
```py
cool_instance = CoolClass()
#Get variable
x = cool_instance.class_variable_1

#Use method
cool_instance.class_method('x')


```

VARIABLES specific to a class object are alled instance variables (opposite to class variables)
Ex.
`cool_instance.fake_key = "This works!"`

Check if an attribute is present:

`hasattr(object, 'attribute')` -> returns boolean

`getattr(object, 'attribute', 'default_value')` -> returns attribute value or default

GET ALL ATTRIBUTES

`dir(cool_class)`




##### Dunder methods
Methods with two underscores before and after the word, behave differently

Example
`__init__` method gets called every time a class is initialised

(!!! This method in theory is called a constructor as it helps initialising the object given that we can pass arguments to it)

```py
class CoolClass:
	def __init__(self, value):
		self.value = value
		pass
	pass
```

TO STRING METHOD

`__repr__` -> returns what is meant to be print when an object is printed (instead of class instance)

ex.

```py
class Employee():
  def __init__(self, name):
    self.name = name
 
  def __repr__(self):
    return self.name
```







































