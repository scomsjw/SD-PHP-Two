# Introduction to MySQL

## If you are using selene

You already have a database set up for you (it's named after your student number). You only have access to this one database.
* Open a web browser, open https://selene.hud.ac.uk/phpmyadmin/
* Enter your
    * username e.g. u0123456
    * password. This should be your date of birth in the form two digits for the day, three letters for the month, and two digits for the year. An example would be **01jan98**.
* Once logged in, on the left-hand side you will see your available database.
* Click on it (its named with your student number).
* Skip down these instructions to 'Creating Tables'

## If you are using XAMPP
You can create as many databases and users as you want. My advice is to create a database and user for this database that mimics the set-up on selene.
* Open the Control Panel
* Start Apache
* Start MySQL
* Open a web browser, open http://localhost/phpmyadmin/ . You won't need to login as you will be logged as the root 'super user' automatically.
* Select 'User Accounts'
* Select 'Add user account'
* Enter the following settings
  * Username: **Use text field** : your student number e.g. **u1234567**
  * Host name: **Local** : **localhost**
  * Password: **Use text field** : Date of Birth e.g. **01jan97**
* Select 'Create database with same name and grant all privileges'
* Don't grant global privileges
* Scroll down and click 'Go'

This will set up a user with your selene login details and a database named with your student number. It will appear on the left-hand side.
* Click on your student number database to open it.

## Creating Tables
* Make sure your database is selected.
* Click on the SQL tab, paste in the following code:

```SQL
CREATE TABLE films (
  id int(11) UNSIGNED NOT NULL AUTO_INCREMENT,
  title varchar(100) NOT NULL,
  year smallint(6) NOT NULL,
  duration smallint(6) NOT NULL,
  CONSTRAINT PRIMARY KEY (id)  
)
```
* Refer back to the notes for an explanation of the SQL statement
* Click 'Go'
* A 'films' table should be created
* Select the table and click 'structure' to check the table you have created

## Inserting Data
* Click on the SQL tab, paste in the following:
```SQL
INSERT INTO films (id, title, year, duration) VALUES (NULL, 'Jaws', 1975, 124)
```
* Click 'browse' you should be able to see the row in the database table
* Click on the SQL tab.
* Write your own INSERT statement to add another film to the table.

## Selecting Data
* We are going to need a few more films
* Click on the SQL tab. Paste in the following:
```SQL
INSERT INTO `films` (`id`, `title`, `year`, `duration`) VALUES
(NULL, 'Winter\'s Bone', 2010, 100),
(NULL, 'Do The Right Thing', 1989, 120),
(NULL, 'The Incredibles', 2004, 115),
(NULL, 'The Godfather', 1972, 177),
(NULL, 'Dangerous Minds', 1995, 99),
(NULL, 'Spirited Away', 2001, 124),
(NULL, 'Moonlight', 2016, 111),
(NULL, 'Life of PI', 2012, 127),
(NULL, 'Gravity', 2013, 91),
(NULL, 'Arrival', 2016, 116),
(NULL, 'Wonder Woman', 2017, 141),
(NULL, 'Mean Girls', 2004, 97),
(NULL, 'Inception', 2010, 108),
(NULL, 'Donnie Darko', 2001, 113),
(NULL, 'Get Out', 2017, 117);
```
* Click 'go'
* Select browse to make sure the import worked.
* Select the SQL tab, enter the following

```SQL
SELECT * FROM films WHERE title="Inception"
```
* Click 'go'. You should see the results of the query.
* Write SELECT statements that will do the following. Refer to the notes for similar examples/advice. 
  * List all the films that were made in 2004
  * List all the films  with a duration of 100 minutes or greater
  * List these films in order, from the longest to the shortest
  * List all the films that have a title that contains the word 'the'
  * List all the films that have a title that starts with the word 'the'
  * List all the films that have a title that contains the word 'the' that weren't made in 2004
  * List all the films except for 'Mean Girls' and 'Get Out' that have a title that contains the letter 'g'
  * List the three shortest films
  * List the 4th and 5th oldest films
  * List the total duration of all the films in the table
  * List all the different years (no duplicates) in the table
  * Display the number of films in the table
  * Display the average duration of all the films in the database
  * Display the average duration of all the films made in the 21st century
  * List each year and the number of films made in that year
  * List each year. For each year, display the average duration of all the films made in that year, order the results in ascending order
  * Modify the above by rounding the duration of each film to the nearest minute (you might have to do some extra research)

## Deleting Data
* Select SQL tab
* Write an SQL statement to delete a film from the database table.


