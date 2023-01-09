# SQL
Structured Query Language

Database: is a set of data stored in a computer, usually structured into tables.

A relational database is a type of DB. It uses a structure that allows us to identify and access data in relation to another piece of data in the database and is often organised into tables. Tables are organised in rows and columns where each row is an observation and each column stores a piece of data with a specific data type.

RDBMS (relational database management system) is a program that allows you to create, update and administer a relational database. (MySQL, PostgreSQL, OracleDB, SQL Server, SQLite, )

SQL (Gets table) -> to CSV -> Pandas (sources data from CSV) -> Matplotlib (plots data)

*Churn rate* is the percent of subscribers to a monthly service who have cancelled (cancellations x 100 / total subscribers)


## Statements
In SQL the inputs are called statements. They're composed of _clause_ (ex CREATE TABLE) (USUALLY CAPITALISED) and _parameters_ (ex column_1)

ex
```sql
CREATE TABLE table_name (
   column_1 data_type, 
   column_2 data_type, 
   column_3 data_type
);
```

#### CREATE
CREATE statements allow us to create a new table in the database, it requires the neame of the table as well as the parameters with their associated data type.

ex:
```sql
CREATE TABLE celebs (
   id INTEGER, 
   name TEXT, 
   age INTEGER
);

```

###### CONSTRAINTS
Constraints are invoked after specifying the data type for a column. They can be used to tell the DB to reject inserted data that does not adhere to a certain restriction. 
ex.
```sql
CREATE TABLE celebs (
   id INTEGER PRIMARY KEY, 
   name TEXT UNIQUE,
   date_of_birth TEXT NOT NULL,
   date_of_death TEXT DEFAULT 'Not Applicable'
);
```

Most common:
- PRIMARY KEY: used to uniquely identify a row. Attempts to insert an identical value in the same column will result in a constraint violation.

- UNIQUE: similar to primary key, restricts duplicates.

- NOT NULL: does not allow insertion of a NULL value.

- DEFAULT: takes an additional argument that is the assumed value for an inserted row if the new row does not specify a value for that column.



#### INSERT
INSERT statement inserts a new row into a table.

ex:
```sql
INSERT INTO celebs (id, name, age)
VALUES (1, 'Justin', 22);
```


#### SELECT
SELECT statements are used to fetch data from a database. Requires the columns and table at the very minimum. It returns a new table called result set.

```sql
SELECT name FROM celebs;
```

 
#### ALTER TABLE
ALTER TABLE statements adds a new column to a table. Existing rows will have NULL values in the new added column

```sql
ALTER TABLE celebs
ADD COLUMN twitter_handle TEXT;
```


#### UPDATE
UPDATE statements can be used to change existing records, it requires a WHERE condition, otherwise it changes the whole table

```sql
UPDATE celebs 
SET twitter_handle = '@taylorswift13' 
WHERE id = 4; 
```



#### DELETE FROM
DELETE FROM Statement deletes one or more rows from a table. NEEDS a WHERE condition or it will delete the whole table. WHERE clause can be matched with IS NULL to delete rows with null values.





## SQL QUERIES

#### AS
When using select, we can give alias to column name using the keyword AS. Best practice is using single quotes.

ex.
```sql
SELECT name AS 'Titles'
FROM movies;
```


#### DISTINCT
DISTINCT is a keyword used to return unique values in the output. It filters out all duplicate values in the specified columns.

ex.
```sql
SELECT DISTINCT tools 
FROM inventory;
```


#### WHERE
WHERE is a clause used to insert constraints when retrieving values. It accepts logical statements. Logical statements include:
- `=` equal to
- `!=` not equal to
- `< > <= >=` greater, less or equal

ex.
```sql
SELECT *
FROM movies
WHERE imdb_rating > 8;
```


#### LIKE
LIKE is an operator used to compare similar values, and used together with the wildcard '`_`'. For example if we want to select all movies that start with 'Se', have 1 character in between and end with 'en' we would:

```sql
SELECT * 
FROM movies
WHERE name LIKE 'Se_en';
```

Wildcards:
`_`: single character
`%`: zero or more characters (ex all that starts with A: A%, or contains m: %m%) (NOT CASE SENSITIVE)


#### IS NULL / IS NOT NULL
These are used because we cannot use = and != when looking for null values


#### BETWEEN
BETWEEN operator is used in a WHERE clause to filter the result set within a certain range. It accepts two values which are either numbers, text or dates.

ex.
```sql
SELECT *
FROM movies
WHERE year BETWEEN 1990 AND 1999;

SELECT *
FROM movies
WHERE name BETWEEN 'A' AND 'J';
```
Quirk: it includes both dates when searching but it would discard any movie that is called J*, it would include 'J' but discard 'Jaws'


#### AND
AND is used in WHERE clauses to combine multiple conditions. All conditions must be respected
ex.

```sql
SELECT * 
FROM movies
WHERE year BETWEEN 1990 AND 1999
   AND genre = 'romance';
```


#### OR
OR is used in where clause to combine multiple conditions. At least ne condition has to be true
ex.

```sql
SELECT *
FROM movies
WHERE year > 2014
   OR genre = 'action';
```


#### ORDER BY
ORDER BY is a statement used to list the data result in a particular order. We can order alphabetically or numberically. Uses keyword *DESC* for descending order or *ASC* for ascending (default). It always has to be after the WHERE.

ex.
```sql
SELECT *
FROM movies
ORDER BY name;
```



#### LIMIT
LIMIT is used to limit the number of results returned. It ALWAYS goes at the very end of the query and is not always supported.

ex.
```sql
SELECT *
FROM movies
LIMIT 10;
```


#### CASE
CASE statement allows us to create different outputs by handling some if-then logic. In the example we want to condense the ratings in movies to three levels. We use both *WHEN* followed by *THEN*, and *ELSE*. The *CASE* Statement is closed by *END*. The column can be given an alias using *AS*

ex.
```sql
SELECT name,
 CASE
  WHEN imdb_rating > 8 THEN 'Fantastic'
  WHEN imdb_rating > 6 THEN 'Poorly Received'
  ELSE 'Avoid at All Costs'
 END AS 'Review'
FROM movies;
```




## Aggregate functions
Aggregates are calculations performed on multiple rws of a table. 

COUNT - SUM - MAX - MIN - AVG - ROUND

#### COUNT
COUNT() can be used to calculate how many rows in a table for a fast analysis. It takes the name of the column as an argument and counts the number of non-empty values.

ex.
```sql
SELECT COUNT(*)
FROM table_name;
```


#### SUM
SUM() is used to add all values in a particular column. It takes the name of the column as argument.

ex.
```sql
SELECT SUM(downloads)
FROM fake_apps;
```


#### MAX/MIN
MAX() and MIN() are used to return the highest and lowest value in a column respectively. They take the name of the column as argument

```sql
SELECT MAX(downloads)
FROM fake_apps;
```



#### AVG
AVG() is used to calculate the average value of a particular column. It takes the name of the column as argument.

ex.
```sql
SELECT AVG(downloads)
FROM fake_apps;
```


#### ROUND
ROUND() is used to round the results of a calculation. It takes the column name and the number of decimal places as argument.

ex.
```sql
SELECT ROUND(price, 0)
FROM fake_apps;
```

#### GROUP BY
GROUP BY is a clause that is used to aggregate functions, it is used in collabration with the SELECT statement to arrange identical data into groups. It comes after any _WHERE_ statements, but before _ORDER BY_ or _LIMIT_.

ex.
```sql
SELECT year,
   AVG(imdb_rating)
FROM movies
GROUP BY year
ORDER BY year;
```

WHEN USING GROUP BY we can refer to the selected columns using the index of selection (starting at 1), for example instead of repeating ROUND(column) we select it directly:

```sql
#Verbose:
SELECT ROUND(imdb_rating),
   COUNT(name)
FROM movies
GROUP BY ROUND(imdb_rating)
ORDER BY ROUND(imdb_rating);

#Shorter:
SELECT ROUND(imdb_rating),
   COUNT(name)
FROM movies
GROUP BY 1
ORDER BY 1;
```


#### HAVING
HAVING is similar to where as it filters based on logical conditions, however it is used to filter groups in tandem with group by. (For example we want groups whose count is over 10). HAVING always comes after _GROUP BY_ but before _ORDER BY_ and _LIMIT_

ex.
```sql
SELECT year,
   genre,
   COUNT(name)
FROM movies
GROUP BY 1, 2
HAVING COUNT(name) > 10;
```

----

## SQL OVER MULTIPLE TABLES
Being SQL a relational DB tables are split to avoid redundancy and re-joined on query. In order to combine tables we use _JOIN - ON_

```sql
SELECT *
FROM orders
JOIN customers
  ON orders.customer_id = customers.customer_id;
```
GIVEN that the column names are repeated, we have to specify which table we're referring to using the dot notation


#### Different types of joining

INNER JOIN: it only returns rows present in both tables

LEFT JOIN: it returns all rows from the left table and only matching rows from the right one (result is going to be equal to all the rows of the left table with additional info in rows matching the right one, all the other values are going to be null)

RIGHT JOIN: same as left join but on the right

CROSS JOIN: it combines all the rows in a table with all the rows in the other, giving all possible permutations (n_rows_table1 x n_rows_table2). Does not require on!

```sql
SELECT shirts.shirt_color,
   pants.pants_color
FROM shirts
CROSS JOIN pants;
```


#### Primary vs Foreign key

Primary keys: are _unique_ _NON NULL_ values that uniquely identify each row in a table. A table cannot have more than a primary key.

Foreign key: are representatives of primary keys in different tables. Foreign keys can be repeated as a primary key can represent more than a foreign key.



#### UNION
We can use UNION to stack a dataset on top of the other:
```sql
SELECT *
FROM table1
UNION
SELECT *
FROM table2;
```

Restrictions: 
- Tables must have the same number of columns
- Columns must have the same data types in the same order as the first table


#### WITH
We can use WITH to combine two tables, one of which is the result of another calculation:
```sql
WITH previous_results (new alias) AS (
   SELECT ...
   ...
   ...
   ...
)
SELECT *
FROM previous_results
JOIN customers
  ON _____ = _____;
 ```
Essentially we're putting a first query inside the parentheses and giving it an alias

