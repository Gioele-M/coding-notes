# Python intermediate


## Mutable default arguments

Functions with default empty MUTABLE arguments (array, sets and dictionaries) could cause issues as they might reference to the same object, for example:

```py

def createStudent(name, age, grades=[]):
    return {
        'name': name,
        'age': age,
        'grades': grades
    }

#Create student has grades which by default respond to an empty array. When we create two students without passing a grade, we expect to both have distinct empty arrays as grades.

chrisley = createStudent('Chrisley', 15)
dallas = createStudent('Dallas', 16)

# That counterintuitively is not the case, if we were to use a function such as 

def addGrade(student, grade):
    student['grades'].append(grade)
    # To help visualize the grades we have added a print statement
    print(student['grades'])

# And then call 
addGrade(chrisley, 90)
addGrade(dallas, 100)


# Instead of getting as output
#[90]
#[100]

#We would get 
# [90]
# [90, 100]

```

This happens because when the function is defined, the expression is evaluated once, and the same pre-computed value is used for each call. 

A WORKAROUND to this is using None as default argument and putting an if statement 

```py
def createStudent(name, age, grades=None):
  if grades is None:
    grades = []
  return {
    'name': name,
    'age': age,
    'grades': grades
  }
```



## Function arguments

There are 3 commont types of function arguments:
- Positional: called by their position in their function definition
- Keyword: called by their name (declaration is the same as positional)
- Default: given default values


## Variable number of arguments: `*args`

When we wamt to feed an unspecified number of arguments to our function we can use the *unpacking operator* `*` usually defined as `*args` (not necessary).

`*` will store all the arguments passed into the function in the form of a _tuple_ (unpack with for loop)

ex.
```py
def my_function(*args):
	print(args)
```

Pass multiple arguments with `*`
ex.
```py
def sum(num1, num2, num3):
  print(num1 + num2 + num3)

my_num_list = [3, 6, 9]

sum(*my_num_list)



#OTHER WITH RANGE
start_and_stop = [3, 6]
range_values = range(*start_and_stop)

#OTHERS
a, *b, c = [3, 6, 9, 12, 15]
print(b) #-> [6,9,12]

my_tuple = (3, 6, 9)
merged_tuple = (0, *my_tuple, 12)
print(merged_tuple) #-> (0, 3, 6, 9, 12)




```


## Variable number of keyword arguments: `**kwargs`

`**kwargs` takes the form of a dictionary with all the keyword argument values passed to it. Given that it is a dictionary we can use the standard dictionary functions such as '.get()' to interact. (kwargs is also a conventional word and can be substituted)

ex.
```py

def arbitrary_keyword_args(**kwargs):
  print(type(kwargs))
  print(kwargs)
  # See if there's an 'anything_goes' keyword arg and print it
  print(kwargs.get('anything_goes'))
 
arbitrary_keyword_args(this_arg='wowzers', anything_goes=101)

## Print
# <class 'dict'>
# {'this_arg': 'wowzers', 'anything_goes': 101}
# 101
```


Pass multiple kwargs through dictionary
```py
numbers  = {'num1': 3, 'num2': 6, 'num3': 9}
 
def sum(num1, num2, num3):
  print(num1 + num2 + num3)
 
sum(**numbers)
```



## Using both args and kwargs

It can be done but in the declaration it has to be followed this schema:
- Standard positional arguments
- args
- Standard keyword arguments
- kwargs

ex.	
```py
def print_animals(animal1, animal2, *args, animal4, **kwargs):
  print(animal1, animal2)
  print(args)
  print(animal4)
  print(kwargs)


print_animals('Snake', 'Fish', 'Guinea Pig', 'Owl', animal4='Cat', animal5='Dog')

```


------



# Names and namespaces

In python, *names* (or symbolic names) are identifiers for objects, these are stored in the *namespace*, a collection of names and object they reference in the format of a dictionary

There are 4 main types of namespaces:

- Built-in: this is the highest level of namespace and encloses functions like print() and str() as well as True and False. Called with `print(dir(__builtins__))`

- Global: exists one leve below the built-in namespace. This includes all non-nested names in the file we are choosing to run the interpreter on. (Basically includes everything declared within the global scope of the script). Called with `print(globals())`. Anytime we use the import statement to bring in a new module into our program, instead of adding every name from that module (such as all the names in the random module) to our current global namespace, Python will create a new namespace for it. This means there might be potentially multiple global namespaces in a single program.

- Local: namespace only existing inside of the function, in existence until the function terminates. Can be accessed with `print(locals())`

- Enclosing: created specifically when we work with nested functions and jusst like with the local namespace it only exists until the function is done executing






-----
Ongoing







































































































































































