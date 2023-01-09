# SQL
(Structured Query Language)
!!! ONLY USE SINGLE QUOTES

## SQL Keywords

- CREATE
	+ CREATE TABLE Persons (
				PersonID int,
				LastName varchar(255),
				FirstName varchar(255),...);

- ALTER
	+ To modify
	+ ALTER TABLE {tablename} ADD {parameter} {varchar()/int/...};
	
- DROP
	+ To remove
	+ DROP TABLE {tablename};
	+ DROP DATABASE {databasename};
	+ ALTER TABLE {tablename} DROP COLUMN {columnname};
	+ ...

- SELECT
	+ SELECT {column}, {column} FROM {table};
	
- INSERT
	+ INSERT INTO {table} ({parameter1}, {par2}, ...) VALUES ({val1}, {val2})
	+ !!! Booleans are null by default, need to be set
	
- UPDATE
	+ UPDATE {tablename}
	  SET {parameter}={value}      
	  WHERE {column} = {value}
	
- DELETE
	+ DELETE FROM {tablename} WHERE {parameter}={value}
	+ !!!
		* You can protect yourself from accidentally DELETE -ing by using BEGIN TRANSACTION [query] and COMMIT if not ROLLBACK
	


## AGGREGATE FUNCTIONS

- COUNT
	+ SELECT COUNT {column name} FROM {table} WHERE ...
- COUNT DISTINCT 
	+ Counts unique instances
- SUM
- AVG
- MAX 
- MIN

ex
SELECT {productname}
FROM {table}
WHERE condition (price > 5)
GROUP BY {category} (region)

!! _WHERE KEYWORD_ IS USED TO LINK TABLES TOO
Ex.

----------------------------------------------!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

### SQL JOINS

- INNER JOIN
	+ Returns values matching in all tables
- LEFT OUTER JOIN
	+ Returns matching values plus all left tables values
- RIGHT OUTER JOIN
	+ Same but for right part
- FULL OUTER JOIN
	+ Returns all values from both tables with no repeats








---

## Postgres demo
`docker run --name auguste-db -e POSTGRES_PASSWORD=password -d postgres`
Copy stuff in folder (?) not sure which folder

```bash
docker run -it \
	-p 3000:5000 \
	-e PORT=5000 \
	--name the-api \
	--mount type=bind,src="$(pwd)",dst=/code \
	-w /code \
	node:12.18.4 \
	bash -c "npm install && npm run dev"
```

(Got lost here, need to watch recording again :( )


---

`docker exec -it {container} psql -U postgres`

psql runs sql

NOT SURE HOW/WHAT DOES but opens prompt with postgres interactive shell

---


------------ 
# Postgres

! *If you don't put semicolumn the command does not run rip :(*

- \i 
	+ \i {directory}/{filename}.sql -> open file

- \l -> show all tables (no need ;) (in interactive mode)

- \c {database} -> swich to database

- \d + {tablename} -> show all column names for table

- \dt -> show tables (\dt+ adds more info)

- CREATE DATABASE {databasename}; -> creates database

- DROP DATABASE {databasename}; -> deletes database

- ALTER DATABASE {databasename} RENAME TO {newname} -> rename database

- CREATE TABLE {tablename} ( //-> Create a table with these parameters
			id serial PRIMARY KEY, //-> serial increments automatically + primary key are unique
			name VARCHAR(20) NOT NULL, //-> chars up to 20 + can't be null
			age INT
			);

- DROP TABLE IF EXISTS {tablename}; -> IF EXISTS is good practice!! 

- SELECT * FROM {tablename}; -> show everything in table

- Add rows to table
	+ INSERT INTO {tablename} ({parameter})
				VALUES ('value');
		* Can add multiple values here (add comma and keep doing VALUES (...))
		* Could add multiple parameters at the same time too adding comma inside () and listing more (needs to have counterpart in values)
		

*Additional operators*

- WHERE -> essential keyword for logical operators

- DISTINCT (next to SELECT) -> retrieves only one value per instance

- ORDER BY -> orders by parameter (growing or alphabetically), can also add secondary parameter to order based on 2

- LIMIT N -> limits results retrieval to n elements

- AND / OR -> logical and/or

- LIKE -> similar to = operator, allows for regular expressions

- ILIKE -> equal to LIKE but makes it not case sensitive!

- AS -> Give alias to parameter (used mainly in FROM eg FROM student AS s)

- JOIN ... ON -> Join tables ON this parameter tablename.column 

---


### Examples queries

ex. Get cat named 'Zelda'
SELECT * FROM cats WHERE name = 'Zelda';
SELECT * FROM cats WHERE NAME ILIKE 'zelda' -> Same query but ILIKE makes it case insensitive

ex. Get name of cats by age
SELECT name FROM cats ORDER BY age;

ex. Complex/nested request, retrieve everything from the table respecting second condition
SELECT * FROM cats
WHERE age = (SELECT max(age) FROM cats) 

ex Combined nested request
SELECT * FROM cats
WHERE (age > 5) AND (name LIKE 'T%') -> Like allows for giving regular expressions

ex. Update value for specific tuple, works in wider contexts depending on WHERE
UPDATE cats 
SET AGE = 4
WHERE name = 'Zelda';

ex. Remove cat based on id -> !!! Put condition on it otherwise it deletes everything
DELETE FROM cats
WHERE id=5

ex. Make table from csv
COPY cats
FROM $str%/code/data/cats.csv$str$   -> $str$ are just string delimiters, can swop for ' '
DELIMITER ',' CSV HEADER; 	-> header is for columns names


ex. Make fresh table, remove if exists first for good practice 
DROP TABLE IF EXISTS trainers;
CREATE table trainers (
	id serial primary key,
	name varchar(50) not null,
	cohort_id int references cohorts(id) not null,
	senior boolean
	);

ALTER TABLE trainers
ALTER COLUMN senior
SET DEFAULT FALSE  // NEED THIS AS BOOLEAN ARE NULL BY DEFAULT

insert into trainers (name,cohort_id)
values (...), (...);


ex. Inner join example
select * from student as s
inner join cohorts _on_ cohorts.id = s.cohort_id
where ... 


---





