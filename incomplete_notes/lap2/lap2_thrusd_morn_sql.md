## SQL

PK - FK definitions



### Build a joined query

SELECT contactName, OrderDate 
FROM Orders // -> table we get orderDate from
JOIN Customers //-> table we get contactName from
ON Orders.CustomerID = Customers.CustomerID //-> linking tables

...
Now you can add all restrictions you need with WHERE

WHERE ...






# NoSQL
Mainly used to store documents




