# SQL
- Structured Query Language
- Works on relational databases
- Each table is a named collection of rows. Each row of a given table has the same set of named columns, and each column is of a specific data type


# PostgreSQL
Relational database system which implements SQL language

*Run postgreSQL as background service*
Start
`brew services start postgresql`
Stop
`brew services stop postgresql`


## Use PostgreSQL in a docker container
- Create container
`docker run --name <new-container-name> -e POSTGRES_PASSWORD=<a-password> -d postgres`

- Create container with volume (to use your data)
`docker run --name <new-container-name> --mount type=bind,source="$(pwd)",dst=<destination> -e POSTGRES_PASSWORD=<a-password> -d postgres`

- Attach container in bash shell (work on container using bash)
`docker exec -it <container-name> /bin/bash`

- Attach container in psql shell (work on db using PostgreSQL CLI)
`docker exec -it <container-name> psql -U postgres`


### Interact with PostgreSQL via terminal
- See all databases
	+ `psql -U postgres -l`
- Create a new database 
	+ `createdb <db-name> -U <pg-username>`
- Delete a database
	+ `dropdb <db-name> -U <pg-username>`
- Access database shell
	+ enter
		* `psql <db-name> -U <pg-username>`
	+ exit
		* `\q`


## SQL CLI commands

- See help options
	+ `\h`

- Run sql file
	+ `\i`

- Show all tables
	+ `\l`
	
- Switch satabase
	+ `\c {database}`

- Show all column names for table
	+ `\d {tablename}`

- Show all tables in database
	+ `\dt`
	+ `\dt+` adds more info


## *Work with tables --- Crud routes*

*Create*

- Create a new table
	+ CREATE TABLE Persons (
				PersonID int,
				LastName varchar(255),
				FirstName varchar(255),...);

- Insert values into table
	+ INSERT INTO {table} ({parameter1}, {par2}, ...) VALUES ({val1}, {val2})
	+ Booleans are null by default, need to be declared as t/f


*Read*

- Read from table
	+ SELECT {column}, {column} FROM {table};


*Update*

- Alter a table structure (eg add column)
	+ ALTER TABLE {tablename} ADD {parameter} {varchar()/int/...};

- Update table parameters
	+ UPDATE {tablename}
	  SET {parameter}={value}      
	  WHERE {column} = {value}


*Delete*

- Delete a table, database, column, specific rows
	+ DROP TABLE IF EXISTS {tablename};
	+ DROP DATABASE {databasename};
	+ ALTER TABLE {tablename} DROP COLUMN {columnname};
	+ DELETE FROM {tablename} WHERE {parameter}={value}
		* You can protect yourself from accidentally DELETE -ing by using BEGIN TRANSACTION [query] and COMMIT if not ROLLBACK



##### Read query examples
- all rows in cats table
`SELECT * FROM cats;`
- all names in cats table
`SELECT name FROM cats;`
- all names of cats older than 7
`SELECT name FROM cats WHERE age > 7;`
- cats called Zelda
`SELECT * FROM cats WHERE name = 'Zelda';`
- cats with a name
`SELECT * FROM cats WHERE name IS NOT NULL;`
- unique ages in cats table in descending order
`SELECT DISTINCT age FROM cats ORDER BY age DESC;`
- highest age value in cats table
`SELECT max(age) FROM cats;`
- name of oldest cat in table
`SELECT name FROM cats WHERE age = (SELECT max(age) FROM cats);`
- cats older than 5 whose name starts with 'T'
`SELECT * FROM cats WHERE (age > 5) AND (name LIKE 'T%');`
- count all cats from same location
`SELECT COUNT(id), location FROM owner GROUP BY location`




## SQL aggreagation functions

- COUNT
	+ SELECT COUNT {column name} FROM {table} WHERE ...
- COUNT DISTINCT 
	+ Counts unique instances
- SUM
- AVG
- MAX 
- MIN

*Example*
```sql
SELECT COUNT(id), location 
FROM owners
GROUP BY location;
```


## Additional operators

- WHERE -> essential keyword for logical operations

- DISTINCT (next to SELECT) -> retrieves only one value per instance

- ORDER BY -> orders by parameter (growing or alphabetically), can also add secondary parameter to order based on 2

- LIMIT N -> limits results retrieval to n elements

- AND / OR -> logical and/or

- LIKE -> similar to = operator, allows for regular expressions

- ILIKE -> equal to LIKE but makes it not case sensitive!

- AS -> Give alias to parameter (used mainly in FROM eg FROM student AS s)

- JOIN ... ON -> Join tables ON this parameter tablename.column 




### SQL JOINS

- INNER JOIN
	+ Returns values matching in all tables
- LEFT OUTER JOIN
	+ Returns matching values plus all left tables values
- RIGHT OUTER JOIN
	+ Same but for right part
- FULL OUTER JOIN
	+ Returns all values from both tables with no repeats

_Example 3 table inner join_

```sql
SELECT owners.name AS owner, cats.name AS cat
FROM ((adoptions
INNER JOIN owners ON adoptions.owner_id = owners.id)
INNER JOIN cats ON adoptions.cat_id = cats.id);

```
