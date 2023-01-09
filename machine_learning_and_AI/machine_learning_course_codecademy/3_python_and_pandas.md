# Python and pandas


## Lambda functions
Lambda functions in python are one-line shorthand for functions

```py
add_two = lambda input : input +2 if input < 1 else input +3

```


## Tuples in python
Data structure that allows to store multiple pieces of data. It's different from a list as it is immutable.
Declare tuple
```py
my_tuple = ('Name', 'Surname', 12)

#Access info same as lists
name = my_tuple[0]

#One element tuple are not recognised, so you declare it with a trailing comma
one_element = (4,)

```


## Datetime
https://www.youtube.com/watch?v=smDBZDvsm0I


## Pipenv
https://www.youtube.com/watch?v=BVr-6Ki96XM&t=8s

-------

# Pandas vs Numpy
Pandas is a library to work with tabular data. Its main data structure are DataFrames, structured like spreadheets.
Numpy instead facilitates operations on large quantities of data. Pandas is built on top of Numpy

## Pandas commands for selection

`import pandas as pd`

*Create dataframe from dictionary*
```py
df1 = pd.DataFrame({
  'Product ID': [1, 2, 3, 4],
  'Product Name': ['t-shirt', 't-shirt', 'skirt', 'skirt'], 
  'Color':['blue', 'green', 'red', 'black']
})
```

*Create dataframe from lists - !!! requires naming the columns separately* 
```py
df2 = pd.DataFrame([
    ['John Smith', '123 Main St.', 34],
    ['Jane Doe', '456 Maple Ave.', 28],
    ['Joe Schmo', '789 Broadway', 51]
    ],
    columns=['name', 'address', 'age'])
```

*Transfer FROM AND TO CSV*
```py
pd.read_csv('my_csv.csv')

pd.to_csv('my_csv.csv')
```

*Preview dataframe*
```py
#first 10 rows
print(df.head())

#summary of data
print(df.info())
```


*Select columns*
Two possible syntaxes
```py
df['column_name'] 
df.column_name #(if ccolumn name does not start with numbers and doesn't contain special characters or spaces)
```

*Select multiple columns*
Make sure it's double brackets!! [[]]
```py
new_df = df[['column1', 'column2']]
```



*Select rows*
Indexing starts at 0
```py
df.iloc[2]
```

*Select multiple rows*
Using slicing
```py
df.iloc[3:]
```


*Select rows with LOGIC*
! .isin()
```py
df[df.column_name == 'desired_column_value']

#ex.
df[df.age==30] #Selects all rows with age 30
# can also use < > != <= >=


# COMBINE MULTIPLE STATEMENTS!
# using parentheses
df[(df.age>30) | (df.name == 'Martha')]


# Check for value against list!
df[df.name.isin(['Martha', 'Sarah'])]
```


*Adjust indexes*

```py
df.reset_index() #adds new indexes and moves the old to a new column called index, returns new df

df.reset_index(drop=True) #removes old indexes and returns new df

df.reset_index(drop=True, inplace=True) #modifies current df
```


## Pandas modify DFs

*Add column*

```py
#For specific values (follows df order)
df['new_column'] = [12, 11, 10 , 1]

#For all values
df['other_column'] = True

#Based on existing columns
df['third_column'] = df.new_column * 2
```


*Modify column values*
In this examples they get capitalised
```py
df['Name'] = df.Name.appy(str.upper)
```


*Apply lambda to a column*
`lambda x: [OUTCOME IF TRUE] if [CONDITIONAL] else [OUTCOME IF FALSE]`

```py
df['Email provider'] = df.Email.apply(lambda x: x.split('@')[-1])
```

*Apply lambda based onto a row*
Requires keyword `axis=1`
```py
df['Price with Tax'] = df.apply(lambda row:
     row['Price'] * 1.075
     if row['Is taxed?'] == 'Yes'
     else row['Price'],
     axis=1
)
```


*Rename columns*
```py
df.columns = ['column1', 'column2']

#To be sure
df.rename(columns={
	'name': 'new_name',
	'surname': 'sium'
	},
	inplace=True #To not create another df
	)

```


## Aggregates in Pandas
Aggregate statistic is a way of creating a single number that describes a group. Common aggregate statistics include mean, median and standard deviation.

*The general command for aggregate statistics is `df.column_name.command()`*
The most common commands:
- mean
- std
- median
- max
- min
- count -> counts n of values in the column
- nunique -> number of unique values
- unique -> list of unique values


*Calculate aggregates based on condition*
USING `groupby()`:
General syntax
`df.groupby('column1').column2.measurement().reset_index()`

You want to add `reset_index()` because groupby creates a new Series, not a DataFrame. This will transform it into a DF and move the indices into their own column


_Calculate grouping by column value_
ex.
```py
grades = df.groupby('student').grade.mean().reset_index()
```



*Calculate complex aggregates with lambda and numpy*
We use the apply() method same as before
ex.
```py
# np.percentile can calculate any percentile over an array of values
high_earners = df.groupby('category').wage
    .apply(lambda x: np.percentile(x, 75))
    .reset_index()
```



*Calculate aggregates using more than a column!*
Simply pass an array to groupby
ex.
```py
df.groupby(['Location', 'Day of Week'])['Total Sales'].mean().reset_index()
```


## Pivoting a table
Pivoting a table is rearraging the data inside it to perform an analysis, for example we have a table with location, day of the week and total sales, but we want a column where each location has a row and each day of the week (as column name) spells a total sale. We would use:

```py
#STANDARD CODE
df.pivot(columns='ColumnToPivot',
         index='ColumnToBeRows',
         values='ColumnToBeValues')


# First use the groupby statement:
unpivoted = df.groupby(['Location', 'Day of Week'])['Total Sales'].mean().reset_index()
# Now pivot the table
pivoted = unpivoted.pivot(
    columns='Day of Week',
    index='Location',
    values='Total Sales')
```



# Pandas working with multiple tables

*Inner merge*
The `.merge()` method looks for columns that are common between two DataFrames and the looks for rows where those column values are the same; it then combines the matching rows into a single row in a new table

ex.
```py
new_df = pd.merge(df1, df2)

#Other method if merging more than 2 DF

big_df = df1.merge(df2).merge(df3)

```


*Merge on specific columns*
Often there are columns named 'id' in both tables, this could confuse the algorithm, so instead of .rename() the columns beforehand, we can do it on merge

```py
pd.merge(
    orders,
    customers.rename(columns={'id': 'customer_id'}))
```

OTHERWISE WE CAN USE `left_on and right_on` (left is the first declared and right is the second one)
Pandas changes the names of identical columns to \_x and \_y, so we can rename with `suffixes`
```py
pd.merge(
    orders,
    customers,
    left_on='customer_id',
    right_on='id',
    suffixes=['_order', '_customer']
)
```


*Outer merge*
If there are missing values when merging two tables, the missing row values will be deleted, to avoid this use keyword `how='outer'`. (it includes rows from both tables even if they don't match). Missing values are filled with None or nan (not a number)

```py
pd.merge(company_a, company_b, how='outer')
```

*Left and Right merge*
Still using how but with keywords 'left' and 'right'. You know what those merges do!
```py
pd.merge(company_a, company_b, how='left')

pd.merge(company_a, company_b, how="right")
```



*Concatenate DataFrames*
Using method .concat([...])
It only works if all of the columns are the same in all of the DataFrames
```py
pd.concat([df1, df2])
```





