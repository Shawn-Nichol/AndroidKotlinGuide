

SQL keywords are not case sensitive, SQL statments should be written in caps. 

## SELECT: 
Extracts data fro a database, the data returned is stored in a result table, calle the result -set

Here column1, column 2 are the fild names of the table you want t select data from. 
```
SELECT column1, column2 FROM table_name
```

DISTNICT:
Only returns ddistnct values.
```
SELECT DISTINCT column1, column2 FROM table_name
```



## WHERE: 
THe WHERE clause is used to filter records, The WEHERE clasue is not only used in SELECT statement, it is also usedin UPDATE, DELETE statement
```
SELECT column1, column2 FROM table_name WHERE conidtion
```

The following selects all customer from the country Mexico
```
SELECT * FROM customers WHERE country='Mexico'
```

SQL requires signle quotes around text values, however numric fields should not be enclosed in quotes. 
```
"SELECT * FROM customers WHERE customerID=1"
```
The following operators can be used in where 
= equal
> Greater than
< Less than 
>= Greater than or equal
<= Less than or equal
<> Not equal. Note in some version of SQL this operator may be written as !=
BETWEEN Between a certain range
LIKE Search for a pattern
IN To specify multiple possible values for a column. 




## SQL AND, OR and NOT operators
The where clause can be combined with AND OR, and NOT operators. The AND and OR opertors are used to filter records based on more than one condition. 
- The AND operator displays a record if all the conditions separted by AND are tue. 
- The OR operator displays a record if any of the conditions separted by OR is TRUE. 
The Note operator displays a record if the confitions is NOT true. 

AND syntax
```
SELECT column1, column2 FROM table_name WHERE conditional1 and condition 2 and condition3
```

OR Syntax
```
SELECT column1, column2 FROM table_name WHERE condition1 or condition2 or condition3
```

NOT syntax
```
SELECT column1, colmun2 FROM table_name WHERE NOT condition
```

You can also combine the operators
```
SELECT * FROM customers WHERE countr='Germany' AND (City='Berlin or City='Munchen')
```



## SQL Order by Keyword
The order by keyword is used to sort the result-set in ascending or descending order. The ORDER by keywor sorts the records in ascending order by default. TO sort the records in descending oreder use DESC keyword.

```
SELECT column1, colmun2 FROM table_name ORDER BY column1, column2, ASC | DESC
```


Order by country, and then by customer name when country has the same name
```
SELECT * FROM Customers ORDER BY Country, CusomerName
```

You can also mix up the ordering in the case
```
SELECT * FROM Customers ORDER BY Country ASC, CusomterName DESC
```


## Insert Ino statement
The INSERT INTO statement is used to insert new records ina table. 
```
INSERT INTO table_name (column1, column2, column3) VALUES (value1, value2, value3)
```
If you are adding values for all the columns of the table, you don't need to specify the column names in the SQL query. Hoever, makes ure the order of the values is the same order as the column in the table. 


## Null Values
What is a null value is a field with no value. If a field in table is optional, it is possible to insert a new record or update a record without adding a vlue to this field. Then the field will be saved with a Null value. 

How to Test for null values?
uset he IS NULL of IS  NOT NUll operators. 

```
SELECT column1 FROM table_name WHERE column1 IS NOT NULL
```

```
SELECT custoemrName, ContactName, Address FROM Customers WHERE Address IS NULL
```

Not Null 
```
SELECT CustomerName, ContactName, Address FROM customers WEhRE ADDRESS IS NOT NULL
```

## SQL Update
The Update staement is used to modify the existing records in a table. 
Update Syntax
```
UPDATE table_name SET column1, column2, column3 WHERE conition
```
If Where clause was omited all enteries in teh table will be updated. 


```
UPDATE Customers SET ContactName = 'Alfred Schmidt', City= 'FrankFurt' WHERE CustoemrID = 1
```

Update Mutiple Records
```
UPDATE customers SET contactName="Juan" WHERE country = 'Mexico'
```

















Updates - updates data in a database
Delete: Deletes data from a database
Insert into: inserts data into a database
CreateDatabase: creates a new database
Alter database: modifies a database
Create table: creates a new table
Alter table: Modifies a table
Drop table: deletes a table
CreateIndex: creates an index
Drop Index: Deletes an index. 


