---
title: Table Queries
layout: article
---


## **Synapse REST API**

### Basics Table Query

Select all columns from the table identified by 'syn123'.

````
select * from syn123
````

Select only two columns identified by 'foo' and 'bar' from the table identified by 'syn'123'.

````
select foo, bar from syn123
````

To refer to a column name that contains spaces or punctuation, the name must be enclosed in double quotes.

````
select "has space" from syn123
````

Any double quote within a column name must be escaped with two double quotes.

````
select "The ""Cool"" name" from syn123
````

### Aggregation Functions

Count the number of rows in table 'syn123'.

````
select count(*) from syn123
````

Get the maximum of all values for the column 'foo' in table 'syn123'.

````
select max( foo ) from syn123
````

Get the minimum of all values for the column 'foo' in table 'syn123'.

````
select min(foo) from syn123
````

Get the average of all values for the column 'foo' in table 'syn123'.

````
select avg(foo) from syn123
````

Get the sum of all values for the column 'foo' in table 'syn123'.

````
select sum(foo) from syn123
````
### Set Selection

The DISTINCT keyword can be used to select all distinct value (the value set) from a column.

````
select distinct foo from syn123
````

The DISTINCT keyword applies to all selected columns, so if more than one column is listed, then the results will be a list of the distinct combinations of all selected columns.

````
select distinct foo, bar from syn123
````

The DISTINCT keyword can be used with set functions. In this example, we will get the count of the distinct values from the foo column.

````
select count(distinct foo) from syn123
````
### Filtering

Select all rows where column foo has a value equal to one.

````
select * from syn123 where foo =1
````

Select all rows where column foo has a string value equal to 'a string', the right-hand-side but be within single quotes (').

````
select * from syn123 where foo = 'a string'
````

Select all rows where column foo has a value greater than one.

````
select * from syn123 where foo > 1
````

Select all rows where column foo have a value greater than -1.98e12

````
select * from syn123 where foo > 1.98e12
````

Select all rows where column foo has a value less than one.

````
select * from syn123 where foo < 1
````

Select all rows where column foo has a value that does not equal one.

````
select * from syn123 where foo <> 1
````

Select all rows where column foo has a value greater than or equal to one

````
select * from syn123 where foo >= 1
````

Select all rows where column foo has a value less than or equal to one.

````
select * from syn123 where foo <= 1
````
Select all rows where column foo has a value equal to one, two, or three.

````
select * from syn123 where foo in (1,2,3)
````

Select all rows where column foo has a value between one and two.

````
select * from syn123 where foo between 1 and 2
````

Select all rows where column foo has a null value.

````
select * from syn123 where foo is null
````

Select all rows where column foo has a value that is not null.

````
select * from syn123 where foo is not null
````

Select all rows where column foo has a value with a prefix of 'bar'. In this example the right-hand-side of the LIKE keyword is a regular expression where the '%' represents one or more characters or even zero characters.

````
select * from syn123 where foo like 'bar%'
````

Select all rows where column foo has a value with a prefix of 'bar'. In this example the right-hand-side of the LIKE keyword is a regular expression where the '_' represents one and only one character.

````
select * from syn123 where foo like 'bar_'
````

Select all rows where double column foo is not a number or plus or minus infinity

````
select * from syn123 where isNan(foo) or isInfinity(foo)
````
The default escape character for LIKE regular expression is the '' character. In this example we want to find all rows such that foo that contain 'bar_' so we will need to escape the '_' character.

````
select * from syn123 where foo like 'bar_'
````

To use a different escape character for LIKE regular expression we must define the escape character. In this example, the '|' will be used as an escape character instead of ''.

````
select * from syn123 where foo like 'bar|_' escape '|'
````

Select all rows where column foo has a value equal to one or column bar has a value equals to two.

````
select * from syn123 where foo = 1 or bar = 2
````

Select all rows where column foo has a value equal to one and column bar has a value equal to two.

````
select * from syn123 where foo=1 and bar =2
````

Predicates can be surrounded by the '(' and ')' to enforce precedence and nesting.

````
select * from syn123 where (foo=1 and bar =2) or foobar = 3
````


### Grouping

Select all rows grouping first by foo, then by bar.

````
select * from syn123 group by foo, bar
````

Grouping can be used in conjunction with aggregation function. In this example, values from the foo column are first grouped, then the average of each group is calculated. For this example, one row will be returned for each group.

````
select foo, avg(foo) from syn123 group by foo
````

### Sorting

Select all columns from the table with the returned row order sorted by the values of the foo column in ascending order.

````
select * from syn123 order by foo asc
````

Select all columns from the table with the returned row order sorted by the values of the foo column in descending order.

````
select * from syn123 order by foo desc
````

Multiple columns can be include in the order by clause. In this example, the returned row order will be sorted first by foo in ascending order followed by bar in descending order.

````
select * from syn123 order by foo asc, bar desc
````




### Pagination

Pagination is used to limit the number of results returned in a single request. In this example, we want the first ten rows that match our query.

````
select * from syn123 limit 10 offset 0
````

If the above query fetches the first page of ten results with indices from zero to nine, the second page of ten rows can be fetched using this query.

````
select * from syn123 limit 10 offset 10
````

The OFFSET element is optional. In this example, LIMIT is used to limit the results to the first 5 rows.

````
select * from syn123 limit 5
````

Pagination parameters should always be at the end of the query

````
select * from syn123 where foo =1 group by bar limit 100 offset 0
````



### Reserved Columns

Every table has at least two reserved columns ROW_ID and ROW_VERSION. The values for these column are automatically managed and can not be directly modified. However, these columns can be used in queries like any other column. In this example, we are selecting all columns for a single row using its ROW_ID.

````
select * from syn123 where ROW_ID = 101
````

In this example we are listing all rows that have a current version number greater than 12. Note: while each row can have multiple versions, only the current version of each row will appear in the index used to support table queries. That means it is not possible to list old version of a row or select a row using an old version number.

````
select * from syn123 where ROW_VERSION > 12
````


