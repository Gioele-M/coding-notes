# Data tying

USual issues when inspecting data
- The columns are mislabelled / don't have variable names
- The dataset has nonsensical data
- The variables are stored in both the columns and rows

## Data Wrangling
Check shape of DF and for missing data
`df.shape`



## Preliminary data cleaning
- Drop duplicates
	+ df = df.drop_duplicates()

- Put everything lowercase
```py
# map() applies the str.lower() function to each of the columns in our dataset to convert the column names to all lowercase
restaurants.columns = map(str.lower, restaurants.columns)
```

- Rename column
```py
# axis=1` refers to the columns, `axis=0` would refer to the rows
# In the dictionary the key refers to the original column name and the value refers to the new column name {'oldname1': 'newname1', 'oldname2': 'newname2'}
restaurants = restaurants.rename({'dba': 'name', 'cuisine description': 'cuisine'}, axis=1)
```

- Check data types
```py
print(restaurants.dtypes)

# Change accordingly

## Check number of unique instances
print(restaurants.nunique())
```


## Missing Data
*Check number of missing values in each column*
```py
# counts the number of missing values in each column 
restaurants.isna().sum() 
```

*Force value to be NaN (in this case latitude outside NY wont be accepted)*
```py
# here our .where() function replaces latitude values less than 40 with NaN values
restaurants['latitude'] = restaurants['latitude'].where(restaurants['latitude'] < 40, np.nan) 
 
# here our .where() function replaces longitude values greater than -70 with NaN values
restaurants['longitude'] = restaurants['longitude'].where(restaurants['longitude'] > -70, np.nan) 
 
# .sum() counts the number of missing values in each column
restaurants.isna().sum() 
```



## Characterising missingness with crosstab
crosstab() computes the frequency of two or more variables. Ex we look at the missingness of the url column with isna(). This will return a boolean, true if it is a nan and false if it is not.
```py
pd.crosstab(
        # tabulates the boroughs as the index
        restaurants['boro'],  
        # tabulates the number of missing values in the url column as columns
        restaurants['url'].isna(), 
        # names the rows
        rownames = ['boro'],
        # names the columns 
        colnames = ['url is na']) 


#prints
#url is na    False    True
#boro        
#Bronx          1      1
#Brooklyn       2      4
#Manhattan     11      2
#Queens         2      2
```
The results can be interpreted as most of restaurants in Manhattan have restaurant links, while most in brooklin do not.



## Removing prefixes
Ex. remove https://www. before websites with str.lstrip()

```py
# .str.lstrip('https://') removes the “https://” from the left side of the string
restaurants['url'] = restaurants['url'].str.lstrip('https://') 
 
# .str.lstrip('www.') removes the “www.” from the left side of the string
restaurants['url'] = restaurants['url'].str.lstrip('www.') 
 ```


## Melt table
```py
annual_wage=annual_wage.melt(
 
      # which column to use as identifier variables
      id_vars=["boro"], 
 
      # column name to use for “variable” names/column headers (ie. 2000 and 2007) 
      var_name=["year"], 
 
      # column name for the values originally in the columns 2000 and 2007
      value_name="avg_annual_wage") 
 
print(annual_wage)
```


--------



# Regular expressions (regex)
A regular expression is a special sequence of characers that describe a pattern of text that should be found, or matched, in a string or document

*Literals*
Simplest text we can matchm this is where a regular expression contains the exact text we want to match. (Silly, if we want to match monkey our expression is 'monkey'. Useless)


*Alternation*
Alternation, performed with 'pipe' _(|)_, allows us to match either the characters before OR after the pipe. (a|b matches a OR bs)


*Character sets*
Character sets, denoted by square brackets _[ ]_, let us match one of the character from a series of characters, for varying spelling. (analy[sz]e matches both the british and american spelling)


*Wildcards*
Wildcards, denoted by dot _(.)_, will match any single character (lettern, number, symbol or whitespace) in a piece of text. The dot will match only 1 character ( ... matches any 3 letters). If we want to match an actual '.' we need to escape it '\.'


*Ranges*
Ranges, denoted by hyphen _(-)_, allow us to specify a range of characters in which we can make the match without having to type each individual character. This can be used for the sets for example: [abc] = [a-c]. We can match all capital letters with [A-Z], lowercase [a-z] or digit [0-9]. We can also have multiple ranges ex any capital or lowercase [A-Za-z].


*Shorthands*
Shorthand character classes represent common ranges and make writing regular expressions much simpler:

- \w: 'word character' class, represents the range [A-Za-z0-9_] and matches any uppercase/lowercase character, digit or underscore

- \d: 'digit character' class, represents the range [0-9] and matches any single digit

- \s: 'whitespace character' class, represents the range [ \t\r\n\f\v], matching a single space, tap, carriage return, line break, form feed or vertical tab

These ALSO have the _negated shorthand class_, making it match any character NOT in that regular shorthand:

- \W: 'non-word character' class, equals [^A-Za-z0-9_]

- \D: 'non-digit character' class, equals [^0-9]

- \S: 'non-whitespace' class, equals [^ \t\r\n\f\v]



*Grouping*
Grouping, denoted by parenthesis _( )_, let us group parts of a regular expression together, and allows us to limit alternation to part of the regex. Ex 'I love (baboons|gorillas)' will match both I love baboons and I love gorillas


*Quantifiers*
Fixed quantifiers, denoted with curly braces _{ }_, let us indicate the exact quantity of a character we wish to match, or allow us to provide a qunatitiy range to match on! ex \w{3} -> matches 3 words character -- \w{4,7} matches a minimum of 4 and maximum of 7 characters


*Optional quantifier*
Optional quantifiers, denoted by question mark _?_, allow us to indicate a character in a regez is optional (it can appear either 0 or 1 time). Ex 'humou?r' matches both spelling of humour and humor. To match the actual question mark you need to escape it \?



*Optional quantifier >=0 Kleene star*
Kleene star, denoted with an asterisk _*_, is also a quantifier that allows matches with the preceding character 0 or more times. Ex `meo*w` will match mew, meow, meoooooooow, etc.


*Optional quantifier >=1 Klenee plus*
Kleene plus, denoted with a plus *+*, is a quantifier that matches the preceding character 1 or more times. Ex 'meo+w' will match meow, meooow, meooooooooow, etc.



*Anchors*
The anchors, denoted by the hat _^_ and dollar sign _$_ , are used to match text at the start and at the end of a string. Ex 
```^hello world$``` matches hello world, but would not match hi, hello world becasue it does not start or end with the requested words














# Cleaning data
Important steps in cleaning the data include
- Diagnosing the 'tidiness' of the data
- Reshaping the data (getting right rows and columns)
- Combining multiple files
- Changing the types of the values (eg str float etc)
- Dropping or filling missing values
- Manipulating strings to represent the data better


### Diagnosing the data
For the data to be tidy we're trying to accomplish:
- Each variable as a separate column
- Each row as a separate observation

Function for diagnosing
- .head() -> Display first 5 rows
- .info() -> Display summary of the table
- .describe() -> display the summary statistics of the table
- .columns -> display the column names
- .value_counts() ->display the distinct values for a column




### Dealing with multiple files
When we have multiple files (ex file1.csv, file2.csv, etc) we can combine them into one table using the module 'glob' and pd.concat(). glob can open bultiple files by using regex matching to get the filenames

```py
import glob
import pandas as pd

files = glob.glob('file*.csv')

df_list = []
for filename in files:
	data = pd.read_csv(filename)
	df_list.append(data)

df = pd.concat(df_list)

```




### Reshaping data
When reshaping our data we want 
- Each variable as a separate column
- Each row as a separate observation

So we're looking to transform something like
```
Account	Checking	Savings
“12456543”	8500	8900
“12283942”	6410	8020
“12839485”	78000	92000
```

Into: (notice how the account numbers are repeated to indicate multiple accounts)
```
Account	Account Type	Amount
“12456543”	“Checking”	8500
“12456543”	“Savings”	8900
“12283942”	“Checking”	6410
“12283942”	“Savings”	8020
“12839485”	“Checking”	78000
“12839485”	“Savings”	920000
```

This type of transformation can be done using pd.melt(). Melt takes in a DF, and the column to unpack with the parameters:
- Frame: the DF you want to melt
- id_vars: the column(s) of the old DF to preserve
- value_vars: the column(s) of the DF that you want to turn into variables
- value_name: what to call the column of the new DF that stores the values
- var_name: what to call the column of the new DF that stores the variables

For the previous example:
```py
df = pd.melt(frame=df, id_vars="Account", value_vars=["Checking","Savings"], value_name="Amount", var_name="Account Type")

#We can then use .columns to rename the columns
df.columns(["Account", "Account Type", "Amount"])
```




### Dealing with duplicates
To check for duplicates, we can use the pandas function .duplicated() which returns a Series, telling us which rows are duplicate rows.
We can also use the pandas .drop_duplicates() function to remove all rows that are duplicates of another
IF we want to remove all duplicates of a specific column we can use the keyword subset=[]

```py
#Check for duplicates, returns a series with booleans in case its duplicate
print(df.duplicated())


# Remove duplicates (The whole row has to be identical for it to be removed) 
df = df.drop_duplicates()


# Remove duplicates specific to a column (even if other rows differ)
df = df.drop_duplicates(subset=['column1'])
```




### Split by index
For example, we have birthdates in the format MMDDYYYY. We can split this into 3 different columns

```py
# Create the 'month' column
df['month'] = df.birthday.str[0:2]
 
# Create the 'day' column
df['day'] = df.birthday.str[2:4]
 
# Create the 'year' column
df['year'] = df.birthday.str[4:]
```


### Split by character
If we have our data formatted in a funky way, ex we have words separated by underscores ( _ ), we can create a new column where we split the data based on the character, then create new columns

```py
# Create the 'str_split' column
df['str_split'] = df.type.str.split('_')
 
# Create the 'usertype' column
df['usertype'] = df.str_split.str.get(0)
 
# Create the 'country' column
df['country'] = df.str_split.str.get(1)
```




### Set types
Data types (dtypes) in pandas:
Float • Int • Bool • Datetime • Timedelta • Category • Object

Inspect DF types with
`print(df.dtypes)`

If for example we have prices in dollars in the format XX$, we cannot really convert it to floats as long as the $ is there. We can use the .replace function and the .to_numeric function to convert it

```py
fruit.price = fruit['price'].replace('[\$,]', '', regex=True)
fruit.price = pd.to_numeric(fruit.price)
```

*You can change the pre-assigned data types with:*
```py
df['column'] = df['column'].astype('string')
```


### Missing values

*Method 1: drop all the rows with missing values*
Will return a DF without incomplete rows
```py
df = df.dropna()


#If we want to remove only rows with NaN in a specific column we use
df = df.dropna(subset=['column1'])
```


*Method 2: fill the missing values with the mean of the column or other aggregate values*
```py
df = df.fillna(value={'column1':df.column1.mean(), 'column2': df.column2.mode()})
```


*Why data might be missing*
- Systematic causes: it was simply never provided in the first place
- Structurally missing: missing bc it makes sense, frequently happens when there are one or more fields depending on one another


*Types of missing data*
- Structurally missing: missing for logical reasons
- Missing completely at random (MCAR): mostly hypothetical, every single data point risks falling in this category by chance
- Missing at random (MAR): more realistic than MCAR, each data in the same observation shares the same risk of being MAR (ex individuals not repoting metrix on themselves for embarassment such as BMI)
- Missing not at random (MNAR): there is reason why the data is missing
	+ Spt it with domain knowledge or analysing the DF an finding patterns




*Deleting missing data*
One of the risks involved in deleting data is that it could introduce bias. Data is safe to delete when
- Is either MAC or MCAR (with an eye out on the size of DF)
- Missing data has low correlation with other features in the data


_Types of deletion_
- Listwise
	+ AKA complete-case analysis, is a techniques in which we remove the entire observation when there is missing data
- Pairwise
	+ Only removes rows when there are missing values in the variables we are directly analysing
- Extra - Dropping variables
	+ If a variable is missing enough data, then we don't know enough about it to use it in our analysis, and we can remove it as any conclusions we draw from it would be unreliable




*Imputation*

_Single imputation in Time-Series_
Single imputation is atechnique used to 'fill the blanks', it involves:

- LOCF
	+ Last observation carried forward. With this we fill the missing value with the previous value. This can be used when we see a relatively consistent pattern that has continued to either increase or decrease over time
	+ In python .ffil()
```py
# Using pandas
# Applying Forward Fill (another name for LOCF) on the comfort column	
df['comfort'].ffill(axis=0, inplace=True)


#Using impyute
# Applying LOCF to the dataset
impyute.imputation.ts.locf(data, axis=0)
```


- NOCB
	+ Next observation carried backward. It solves the imputation in the opposite direction of LOCF
	+ In python .bfill()

```py
#pandas
df['comfort'].bfill(axis=0, inplace=True)

#impyute
impyute.imputation.ts.nocb(data, axis=0)
```


- BOCF
	+ Baseline observation carried forward. Used specifically in medical studies. When an observation is missing, it gets replaced with the baseline value
```py
# Isolate the first (baseline) value for our data
baseline = df['concentration'][0]
 
# Replace missing values with our baseline value
df['concentration'].fillna(value=baseline, inplace=True)
```

- WOCF
	+ Worst observation carried forward. If we want to assume its going bad and we're recording improvements
```py
# Isolate worst pain value (in this case, the highest)
worst = df['pain'].max()
 
# Replace all missing values with the worst value
df['pain'].fillna(value=worst, inplace=True)
```




*Multiple imputation*
Is a technique for filling the missing data, in which we replace the missing data multiple times (useed in particular when we have missing data across multiple categorical columns). dter we tried different values, we use an algorithm to pick the best values to replace ur missing data.
Multiple imputation is best for MAR data. 
Using IterativeImputer module in sklearn.

```py
import numpy as np
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer
import pandas as pd
 
# Create the dataset as a Python dictionary
d = {
    'X': [5.4,13.8,14.7,17.6,np.nan,1.1,12.9,3.4,np.nan,10.2],
    'Y': [18,27.4,np.nan,18.3,49.6,48.9,np.nan,13.6,16.1,42.7],
    'Z': [7.6,4.6,4.2,np.nan,4.7,8.5,3.5,np.nan,1.8,4.7]
}
 
dTest = {
    'X': [13.1, 10.8, np.nan, 9.7, 11.2],
    'Y': [18.3, np.nan, 14.1, 19.8, 17.5],
    'Z': [4.2, 3.1, 5.7,np.nan, 9.6]
}
 
# Create the pandas DataFrame from our dictionary
df = pd.DataFrame(data=d)
dfTest = pd.DataFrame(data=dTest)
 
# Create the IterativeImputer model to predict missing values
imp = IterativeImputer(max_iter=10, random_state=0)
 
# Fit the model to the test dataset
imp.fit(dfTest)
 
# Transform the model on the entire dataset
dfComplete = pd.DataFrame(np.round(imp.transform(df),1), columns=['X','Y','Z'])
 
print(dfComplete.head(10))
```






















