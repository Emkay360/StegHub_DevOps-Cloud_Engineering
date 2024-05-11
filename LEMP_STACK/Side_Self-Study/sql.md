## Structured Query Language (SQL)
## Introduction
SQL is a programming language that allows users to store, retrieve, update, and manage data in a rational database. Rational databases use tables to store data with rows and columns representing data attributes and relationships.

### Basic SQL Syntax
SQL syntax is a unique set of rules and guidelines for writing SQL statements. This tutorial gives you a quick start with SQL by listing all the basic SQL Syntax.

All the SQL statements start with any of the keywords like SELECT, INSERT, UPDATE, DELETE, ALTER, DROP, CREATE, USE, SHOW, and all the statements end with a semicolon (;).

- **SELECT clause:** The SELECT clause in SQL is used to specify the columns in a SELECT statement. It is used to query a database and retrieve specific data from one or more tables in the database. The syntax for the SELECT clause is as follows:
```
SELECT column, column1, ...
FROM table_name
```
- **WHERE Clause:** The WHERE clause in SQL filters the rows returned by a SELECT, UPDATE, or DELETE statement. The WHERE clause is used to specify a condition that must be true for a row to be included in the result set or affected by the statement. The basic syntax of a WHERE clause is:
```
SELECT column, column1, ...
FROM table_name
WHERE condition;
```
- **AND/OR Clause:** The SQL AND and OR operators are used in the WHERE clause to combine multiple conditions in a query. The AND operator returns rows that satisfy both conditions, while the OR returns rows that satisfy either. The basic syntax of a WHERE clause with AND and OR operators is:
```
SELECT column, column1, ...
FROM table_name
WHERE condition AND/OR condition1 AND/OR ...;
```
- **ORDER BY Clause:** The SELECT statement in SQL retrieves data from one or more tables in a database. The ORDER BY clause sorts the result set by one or more columns. The basic syntax of a SELECT statement with an ORDER BY clause is:
```
SELECT column, column1, ...
FROM table_name
ORDER BY column [ASC | DESC], column1 [ASC | DESC], ...;
```
- **INSERT INTO Clause:** The INSERT INTO clause in SQL adds new rows of data to a table. The basic syntax of the INSERT INTO clause is:
```
INSERT INTO table_name (column, column1, ...)
VALUES (value, value1, ...);
```
- **UPDATE Clause:** The UPDATE clause in SQL is used to modify existing data in a table. It is typically used in conjunction with a SET clause to specify the new values for the columns and a WHERE clause to identify the rows that should be updated. The basic syntax of the UPDATE clause is:
```
UPDATE table_name
SET column = value, column1 = value1, ...
WHERE condition;
```
- **DELETE Clause:** The DELETE clause in SQL is used to delete existing rows of data from a table. It is typically used in conjunction with a WHERE clause to specify the rows that should be deleted. The basic syntax of the DELETE clause is:
```
DELETE FROM table_name
WHERE condition;
```

### Syntax Basic Rules
1. SQL syntax has a set of rules that must be followed for an SQL statement to be valid. Here are some of the most important rules to keep in mind when writing SQL statements:

2. SQL statements always start with the keywords.
3. SQL keywords must be written in uppercase. For example, SELECT, FROM, WHERE, etc.
4. SQL statements must end with a semicolon (;).
5. Table and column names must be written in lowercase; if multiple words are used, they should be separated by an underscore (_) or camelCase.
6. String values must be enclosed in single quotes (' ').
7. Date values must be enclosed in single quotes (' ') and should be in the format 'YYYY-MM-DD'.
8. Numeric values should not be enclosed in quotes.
9. Each clause in a SQL statement should be written on a separate line for better readability.
10. The SELECT clause must come first in a SELECT statement, followed by the FROM clause and other clauses such as WHERE, JOIN, GROUP BY, and ORDER BY.
11. When using the WHERE clause to filter results, the comparison operator must be placed between the column name and the value being compared.
12. When joining tables, the ON clause should specify the relationship between the columns of the two tables.
13. SQL is not case-sensitive, which means the update keyword is the same as the UPDATE.
