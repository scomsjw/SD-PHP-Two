# Overview of SQL

We use a language called SQL (Structured Query Language) to issue commands to the RDMS (MySQL). We can issue these commands in a number of different ways:
* From PHP code
* Using a web interface – phpmyadmin
* Using a visual web interface - phpmyadmin

## Creating Tables
Here's an empty database table anmed *students*.

| id | last_name | first_name | course | mark |
|----|-----------|------------|--------|------|
|    |           |            |        |      |
|    |           |            |        |      |
|    |           |            |        |      |
|    |           |            |        |      |
|    |           |            |        |      |
|    |           |            |        |      |

To create this table we use the SQL command CREATE TABLE it has the following general structure:
```sql
CREATE TABLE  tablename(
column1name type (size) other info,
column2name type (size) other info,
)
```

For the *students* table:
```sql
CREATE  TABLE students
(
id INT UNSIGNED NOT  NULL  AUTO_INCREMENT,
last_name VARCHAR(30 )  NOT  NULL ,
first_name VARCHAR(20)  NOT  NULL ,
course VARCHAR(30)  NOT  NULL ,
mark TINYINT UNSIGNED NOT  NULL,
CONSTRAINT PRIMARY KEY (id)
);

```
* CREATE TABLE is an SQL command
* Next comes the name we give the table, in this case *students*
    * Name all tables and columns in lower case (some RDBMS are case-sensitive)
* Then comes a set of brackets
* The individual columns are defined inside these brackets e.g. the *id* is defined by the following SQL snippet.

```sql
id INT UNSIGNED NOT NULL AUTO_INCREMENT,
```
* The name of the column is followed by the type of data the column will contain and the size of the column data (in this example INT).
* UNSIGNED means we only allow positive integers.
* NOT NULL means every row must have a value for this column.
* AUTO_INCREMENT. When we insert data, if we leave this field blank MySQL will automatically generate a unique number. Each id value needs to unique because it is the primary key.

The other columns follow a similar pattern but with different data types.

At the end of the CREATE TABLE statement we specify the id column is the primary key:
```sql
CONSTRAINT  PRIMARY KEY (id)
```


## Inserting Data
General syntax
```sql
INSERT INTO tablename (column1, column2, column3 etc.)
VALUES (value1,value2,value3 etc.)
```
An example for the students table
```sql
INSERT INTO students (id, last_name, first_name, course, mark)
VALUES (NULL , 'Compton', 'Jane', 'IT', 58);
```
Would result in a table that looks like:

| id      | last_name     | first_name | course    | mark     |
|---------|---------------|------------|-----------|----------|
|    1    |    Compton    |    Jane    |    IT     |    58    |
|         |               |            |           |          |
|         |               |            |           |          |

* Note that NULL was entered for the id value
    * We specified AUTO_INCREMENT for this column so MySQL automatically generates the next available unique id number (1 in this case).

We can insert multiple rows e.g.
```sql
INSERT INTO students
(id, last_name, first_name, course, mark)
VALUES
(NULL, 'Atherton', 'Ian', 'Computing in Business', 32),
(NULL, 'Hutton', 'Kate', 'Web Design', 72),
(NULL, 'Miandad', 'Yousuf', 'IT', '61'),
(NULL, 'Laxman', 'Sunil', 'Web Technologies', '52'),
(NULL, 'Crowe', 'Grace', 'Computing', '70');
```
Would change my table to look like the following:

| id      | last_name      | first_name   | course                        | mark     |
|---------|----------------|--------------|-------------------------------|----------|
|    1    |    Compton     |    Jane      |    IT                        |    58    |
|    2    |    Atherton    |    Ian       |    Computing   in Business    |    32    |
|    3    |    Hutton      |    Kate      |    Web   Design               |    72    |
|    4    |    Miandad     |    Yousef    |    IT                        |    61    |
|    5    |    Laxman      |    Sunil     |    Web Programming         |    52    |
|    6    |    Crowe       |    Grace     |    Computing                  |    70    |

## Getting Data Out - SELECT

Here's an example SELECT statement:
```SQL
SELECT first_name, last_name FROM students;
```
This would return a result set that looks like:

| last_name      | first_name   |
|----------------|--------------|
|    Compton     |    Jane      |
|    Atherton    |    Ian       |
|    Hutton      |    Kate      |
|    Miandad     |    Yousef    |
|    Laxman      |    Sunil     |
|    Crowe       |    Grace     |

We specify which columns we want to include in the output (in this case *first_name* and *last_name*)

### Selecting everything
We use an asterix (\*) to select all columns e.g.
```sql
SELECT * FROM students
```
Would return:

| id      | last_name      | first_name   | course                        | mark     |
|---------|----------------|--------------|-------------------------------|----------|
|    1    |    Compton     |    Jane      |    IT                        |    58    |
|    2    |    Atherton    |    Ian       |    Computing   in Business    |    32    |
|    3    |    Hutton      |    Kate      |    Web   Design               |    72    |
|    4    |    Miandad     |    Yousef    |    IT                        |    61    |
|    5    |    Laxman      |    Sunil     |    Web Programming         |    52    |
|    6    |    Crowe       |    Grace     |    Computing                  |    70    |

### The SELECT Statement - DISTINCT
Imagine we wanted a list of all courses that take the module.

```sql
SELECT DISTINCT course FROM students
```

| course                        |
|-------------------------------|
|    IT                         |
|    Computing   in Business    |
|    Web   Design               |
|    Web   Programming          |
|    Computing                  |

The DISTINCT keyword  means we don't get duplicate values in the returned results

There are two students on the IT course, but IT only appears once in the returned results.

### The SELECT Statement - WHERE
We can choose to retrieve specific rows using a WHERE clause e.g. I want to know the names of all the students on the IT course.

```sql
SELECT first_name, last_name FROM students WHERE course="IT"
```
Would return

| first_name   | course   |
|--------------|----------|
|    Jane      |    IT    |
|    Yousef    |    IT    |

There are other conditional operators we can use:
* = Equal to
* < Less than
* \> Greater than
* <= Less than or equal to
* \>= Greater than or equal to
* <> Not equal to

For example, imagine we wanted a list of everyone who has passed.
```sql
SELECT first_name, last_name
FROM students
WHERE mark >= 40
```
| last_name     | first_name   | mark     |
|---------------|--------------|----------|
|    Compton    |    Jane      |    58    |
|    Hutton     |    Kate      |    72    |
|    Miandad    |    Yousef    |    61    |
|    Laxman     |    Sunil     |    52    |
|    Crowe      |    Grace     |    70    |

Everyone apart from Ian Atherton is selected.

### The SELECT Statement - LIKE
The LIKE operator works with strings and allows for 'fuzzy matching'. The % sign is used as a wildcard.

```sql
SELECT * FROM students WHERE last_name LIKE '%xyz';
```
Would match all strings that end in 'xyz' e.g. 'wxyz' or 'some text xyz'.

```sql
SELECT * FROM students WHERE last_name LIKE 'abc%';
```
Would match all strings that start with 'abc' e.g.'abcd' or 'abc some text'.
```sql
SELECT * FROM students WHERE last_name LIKE '%abc%';
```
Would match all strings that start, contain, or end with the string 'abc' e.g.'abcd', 'abc some text' or 'something abc' or 'something abc else'.

Imagine I want to know the last name and mark of all the students studying Computing or Computing in Business. The LIKE clause allows us to select any students with the text 'Computing' somewhere in the course title.

```sql
SELECT last_name, mark FROM students WHERE course LIKE '%Computing%'
```
| last_name      | mark     |
|----------------|----------|
|    Atherton    |    32    |
|    Crowe       |    70    |

### The SELECT Statement - IN
We can test against a set of values using IN e.g. if I wanted the details of all the students on Web Design or Web Programming
```sql
SELECT last_name, course, mark FROM students
WHERE course IN ("Web Design", "Web Programming")
```
| last_name    | course                  | mark     |
|--------------|-------------------------|----------|
|    Hutton    |    Web   Design         |    72    |
|    Laxman    |    Web   Programming    |    52    |

We can also use NOT IN e.g.

```sql
SELECT last_name, course, mark FROM students
WHERE course NOT IN ("Web Design", "Web Programming")
```
| last_name      | course                        | mark     |
|----------------|-------------------------------|----------|
|    Compton     |    IT                         |    58    |
|    Atherton    |    Computing   in Business    |    32    |
|    Miandad     |    IT                         |    61    |
|    Crowe       |    Computing                  |    70    |

### The SELECT Statement – AND and OR
We can use the AND and OR operators to perform two or more tests in a single statement e.g. If I want to know the details of all the IT students with a mark over 59.

```sql
SELECT * FROM students
WHERE course = "IT" AND mark >59
```
| id      | last_name      | first_name   | course                        | mark     |
|---------|----------------|--------------|-------------------------------|----------|
|    4    |    Miandad     |    Yousef    |    IT                        |    61    |

### The SELECT Statement - ORDER
The ORDER BY clause is used to sort the output and present it in either ascending or descending order. The default is ascending order. We can order using numbers or text.
```sql
SELECT * FROM students ORDER BY mark DESC
```
| id      | last_name      | first_name   | course                        | mark     |
|---------|----------------|--------------|-------------------------------|----------|
|    3    |    Hutton      |    Kate      |    Web   Design               |    72    |
|    6    |    Crowe       |    Grace     |    Computing                  |    70    |
|    4    |    Miandad     |    Yousef    |    IT                        |    61    |
|    1    |    Compton     |    Jane      |    IT                        |    58    |
|    5    |    Laxman      |    Sunil     |    Web Programming         |    52    |
|    2    |    Atherton    |    Ian       |    Computing   in Business    |    32    |

### The SELECT Statement - LIMIT and OFFSET
We can limit the number of returned results. This is useful if we have a very large table or if we are doing pagination. In the following example only the first three results are returned.

```sql
SELECT * FROM students
ORDER BY mark DESC
LIMIT 3
```

| id      | last_name      | first_name   | course                        | mark     |
|---------|----------------|--------------|-------------------------------|----------|
|    3    |    Hutton      |    Kate      |    Web   Design               |    72    |
|    6    |    Crowe       |    Grace     |    Computing                  |    70    |
|    4    |    Miandad     |    Yousef    |    IT                        |    61    |

OFFSET is used to specify the starting point for returning rows. For example in the following query we skip the first two rows and show results from the third row onwards.
```sql
SELECT * FROM students
ORDER BY mark DESC
LIMIT 3 OFFSET 2
```
| id      | last_name      | first_name   | course                        | mark     |
|---------|----------------|--------------|-------------------------------|----------|
|    4    |    Miandad     |    Yousef    |    IT                        |    61    |
|    1    |    Compton     |    Jane      |    IT                        |    58    |
|    5    |    Laxman      |    Sunil     |    Web Programming         |    52    |

### The SELECT Statement - COUNT and GROUP BY
The COUNT function tells us how many rows have been found. The AS keyword is used to specify a name for the new column.
```sql
SELECT COUNT(*) AS 'Number of students'
FROM  students
```
| Number of students      |
|---------|
|    6    |

We can group rows together and count the size of the groups.
```sql
SELECT course, COUNT(course) as "Number of students"
FROM  students
GROUP BY course
```
|course| Number of students|
|------|---------|
|IT|    2    |
|Computing in Business|    1   |
|Web Design|    1    |
|Web Technologies|    1    |
|Computing|    1    |

### The SELECT statement – Aggregate functions
The AVG(), MIN(), MAX() and SUM() functions work with data that has been grouped. For example
```sql
SELECT AVG (mark) AS 'Average Mark'
FROM  students
```
| Average Mark      |
|---------|
|    57.5000    |

Here's another example. This SQL statement shows the highest mark for each course.
```sql
SELECT course, MAX(mark) AS 'Highest Mark' FROM `students` GROUP BY course
```
|course| Highest Mark|
|------|---------|
|IT|    61    |
|Computing in Business|   32   |
|Web Design|    72    |
|Web Technologies|    52    |
|Computing|    70    |

### The SELECT Statement - Combining Features
Many of these operators and clauses can be used in combination with each other. For example:
```sql
SELECT * FROM students
WHERE mark > 50 AND course NOT IN("IT", "Computing")
ORDER BY last_name ASC
LIMIT 1
```
| id      | last_name      | first_name   | course                        | mark     |
|---------|----------------|--------------|-------------------------------|----------|
|    3    |    Hutton      |    Kate      |    Web   Design               |    72    |

## Updating Records
Imagine Ian Atherton has transferred course from Computing in Business to IT. Using the UPDATE statement we can change the value of the course field.
```sql
UPDATE students SET course= 'IT' WHERE id = 2
```
| id      | last_name      | first_name   | course                        | mark     |
|---------|----------------|--------------|-------------------------------|----------|
|    1    |    Compton     |    Jane      |    IT                        |    58    |
|    2    |    Atherton    |    Ian       |    IT   |    32    |
|    3    |    Hutton      |    Kate      |    Web   Design               |    72    |
|    4    |    Miandad     |    Yousef    |    IT                        |    61    |
|    5    |    Laxman      |    Sunil     |    Web Programming         |    52    |
|    6    |    Crowe       |    Grace     |    Computing                  |    70    |

## Deleting a Record
Using the DELETE statement we can remove rows from a table
```sql
DELETE FROM students WHERE id = 4
```
| id      | last_name      | first_name   | course                        | mark     |
|---------|----------------|--------------|-------------------------------|----------|
|    1    |    Compton     |    Jane      |    IT                        |    58    |
|    2    |    Atherton    |    Ian       |    IT   |    32    |
|    3    |    Hutton      |    Kate      |    Web   Design               |    72    |
|    5    |    Laxman      |    Sunil     |    Web Programming         |    52    |
|    6    |    Crowe       |    Grace     |    Computing                  |    70    |
