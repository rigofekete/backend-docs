# RELATIONAL DATABASES

<br>

## What is a relational database

Type of database meant to store data in `tables` so that it can have a `relationship` with data from other `tables`. 

A `relationship` between `tables` assumes that one of these `tables` has a `foreign key` that `references` the `primary key` of another `table`.


A `relational database` stores data in tables and links them using keys, enforcing relationships and constraints while supporting `CRUD` operations.


These 'CRUD' operations on the database are executed with code, more specifically, `queries`. There are different `query` languages available for developers to work with databases, like `SQL` systems and non `SQL` systems.

Relational databases, data is structured in the following way:

- `Tables` representing data.
- Each table has `columns` or `fields`.
- Contains `rows`, which can be called `records` or `entries`
- Each `record` normally has a `primary key` (unique `id`)

<br>

<img src="images/relational_db.png" alt="relational db" width="500">

<br>

## Migrations

A `migration` is an incremental updated version ('up' migration) of the initial table structure (`schema`). This is done to allow the `tables` to be scalable, in steps, while tracking each version of its previous state. This is very important because we can easily revert the state of the `tables` to previous versions, by way of a `down` migration. 

Another important aspect of executing a `migration` on a `table`, is to carefully check the current queries (code), and update them if necessary, to avoid having them accessing columns that might not exist any longer. 

A good `migration` should be created in a way which will allow it to be `reversible`.

<br>

## Null values 

A 'null' value is a cell that does not hold any value. A column can have a `constraint` that specifies if the value can or cannot be 'null'.

<br>

## Constraints

An rule set to columns, while defining the schema, explicitly limiting them to a given `constraint`. For example, if we add a `name` column to a table and set it to have a 'NOT NULL' constraint, we guarantee that the value received through the query will never be `null`. 

<br>

## Primary key

A special column that is set to each `row` of a `table`, guaranteeing their unique identification. Mostly used as a `constraint` on an `ID` column. This is crucial to protect each record uniqueness while interacting with other tables. 

<br>

## Foreign keys

Essential to create relationships between tables. A foreign key, is a named constraint created for a column (or columns), forcing referential integrity (concept of ensuring that the referenced value exists) by requiring that the value set or received matches a valid primary key (or unique key) from the given referenced table. 

```sql
rental_id INTEGER,
-- defining the name of the constraint explicitly is not mandatory. 
-- If no name is defined for a constraint, the engine will set up a default name for it.   
CONSTRAINT fk_rentals
FOREIGN KEY (rental_id)
REFERENCES rental(id);
```

This will ensure that the `foreign key`, `rental_id`, will contain a valid key from the `rental` table. 

<br>

## Schema 

The blueprint structure of the created database. It defines all of the aspects of how the data is organized, from the tables themselves, to the types of each column, their constraints and how the tables are related to each other, through their references (foreign keys). 

<br>

## ORM 

`Object-Relational Mapping` is a tool that maps database records to in-memory objects. The big benefit of its use is that it allows `CRUD` operations to be executed with more standard programming languages, instead of manually typing queries every time we need to interact with the database. 

For example, a `table` named `user` from a relational database, can be represented in code as:

```go
type User struct {
    name string
    age int
}
```

The type `User`, is the `table` `User` from the database and each of its instances is a `record` (`row`) of that `table`.

`ORM` will also allow functions to be created in order to interact with the object, which will subsequently update the actual data living in the relational database. 

<br>

## Aggregations

A single value derived from a combination of multiple other values from a table.  

The `COUNT` statement, for example, is an aggregate function:

```sql
SELECT COUNT(*) FROM tapes
WHERE name = $1;
```

`Aggregations` are achieved through the use of "aggregators", which are `aggregate` functions, like `COUNT`, that return a single value resulting from the computation of multiple other values.

Besides `COUNT`, other `aggregate` functions exist, such as:

`SUM`, `AVG`, `MAX`, `MIN`, `ROUND`


#### GROUP BY

Aggregations can return multiple values when combining the queries with the `GROUP BY` clause. When used, this clause will make the query output the aggregation of each record, belonging to the specified group:

```sql
SELECT album_id, count(song_id) AS song_count
FROM songs
GROUP BY album_id;
```

This query, would generate a table with the count of each `song_id` per `album_id`, since it is instructed to `GROUP BY` album_id. 

#### HAVING

Keyword that acts on the result of the `GROUP BY` clause, filtering its outcome. 

It is similar to `WHERE` but the crucial difference lives in the fact that it filters the results grouped by the `GROUP BY`. 

- A `WHERE` condition is applied to all the data in a query before it's grouped by a `GROUP BY` clause.

- A `HAVING` condition is only applied to the grouped rows that are returned after a `GROUP BY` is applied.

Example of a query:

```sql
SELECT album_id, count(id) as count
FROM songs
GROUP BY album_id
HAVING count > 5;
```

<br>

## Subqueries

Calling a query from within the main query, whenever additional specific data is required. For example:

```sql
SELECT id, song_name, artist_id
FROM songs
WHERE artist_id IN (
    SELECT id
    FROM artists
    WHERE artist_name LIKE 'Rick%'
);
```

Here, the query is returning rows with `id`, `song_name` and `artist_id`, from the `songs` table, where `artist_name` starts with the "Rick" substring. 

In the example, we are using the `artist_id` `foreign key`, which references the `artists` table `id`, allowing the subquery to compute a set of `ids` from a different table, containing the `artist_name` column, so that the main query can finally filter which rows should be returned.

It is important to note that the use of nested queries is not only restricted to the use of foreign keys, like the example above. Any additional data operation can be done through subqueries.

<br>

## Normalization

### Types of relationships

#### One to One

A `record` on a table that has a single relationship with a `record` from another `table`. For example, a `user` from a `User` table has `1` single email address from an `Emails` table, and that single email address has only `1` `user` linked to it. 

This is achieved by making the `Email` table have a `foreign key` on its `unique` `email_id`, referencing the `user` `id`. 

#### One to Many

Occurs when a single `record` on a `table` has multiple relationships with other `records`. For instance, a `customer`, from a `customers` table, can have multiple `orders`. It is important to note that this `one-to-many` relationship only has one possible direction. The single `order` cannot have multiple `customers`. 

To establish this type of relationship, a `foreign key` (`customer_id`) is used in the `orders` table, to reference the `id` of the `customer` table, restricting the `orders` table to only have 1 `customer` per `order`, but allowing the `customer` to have multiple `orders`. 

The difference with the `one-to-one` example is that the `foreign key` of the `orders` table (on the `many` side) is not `unique`.

```sql
CREATE TABLE customers (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE orders (
    id INTEGER PRIMARY KEY,
    amount INTEGER NOT NULL,
    customer_id INTEGER,
    CONSTRAINT fk_customers
    FOREIGN KEY (customer_id)
    REFERENCES customers(id)
);
```

#### Many to Many

Multiple `records` in a table can be related to multiple `records` in another `table`.

Again, using the image from the beginning of the document:

<br>

<img src="images/relational_db.png" alt="relational db" width="500">

<br>

This is an example of a `many-to-many` relationship. Many `Students` can have many `Courses`, and, on the opposite direction, many `Courses` can have multiple `Students`. 

As seen in the example, this is achieved with the creation of another `table`, called a `joining table`. 

A `joining table` is represented by the `StudentCourses` visible in the image. It contains a `non unique` `id`, `StudentID`, and a `non unique` `id`, `CourseId`. Now, the thing that makes this `many-to-many` relationship work is that the combination of `StudentID` and `CourseID` is `unique`, defined in the schema like this:

```sql
CREATE TABLE student_courses (
student_id INT,
course_id INT,
UNIQUE(student_id, course_id)
FOREIGN KEY (student_id) REFERENCES students(id),
FOREIGN KEY (course_id) REFERENCES course(id)
);
```

This means that only 1 `record` can have that exact combination of ids, but, and this is crucial, multiple combinations between the 2 are allowed. 

This `unique` combination of `student_id` and `course_id` is the `primary key` of this `table`. 

### Database Normalization

Method for structuring the database schema in order to improve `data integrity` and `data redundancy`. 

#### Data integrity 

Accuracy and consistency of the data. For example, storing the `age` of a user, instead of a `date of birth` is an example of an inaccurate way of dealing with this data. With the passage of time, this data would be absolutely inaccurate. Storing the actual `date of birth` would keep this information consistent and correct through the passing of time. 

The rule of thumb for data integrity is to store raw values and compute them when necessary, and never store pre-computed values. 

#### Data Redundancy

Storing the same data multiple times, in different places. For example, storing the email of a user in 2 different tables. 

This can be very serious in situations where the data is changed in one place, but all of the other places where it was store redundantly, remain invalid or outdated.  

<br>

### Normal Forms

<br>

<img src="images/normal forms.png" alt="normal forms" width="400">

<br>

A database can adhere to different "normal forms", each one of them describing how `normalized` they are, by way of a set of premisses. 

`Normalized` here meaning, categorizing the level of `integrity` and `duplication` of data in a database.

The more `normalized` the data is, the more integrity it has, as well as less duplicated data. 

`BCNF` (`Boyce-Codd`) is the most `normalized` a database can get and `NF1`, the least. 

#### 1st Normal Form (1NF)

To be complaint with fist normal form, a database should follow these rules:

- It must have a `unique` `primary key`.
- A cell cannot have a nested table as its value.

#### 2nd Normal Form (2NF)

A table in second normal form follows all the rules of 1st normal form, and one additional rule which only applies to composite primary keys:

- All `columns` that are not part of the `primary key` are dependent on the `entire` `primary key`, and not just one of the `columns` in the `primary key`.

#### 3rd Normal Form (3NF)

A table in 3rd normal form follows all the rules of 2nd normal form, and one additional rule:

- All `columns` that aren't part of the `primary key` are dependent solely on the `primary key`.

The main difference between `2NF` and `3NF` is that the former allows `columns` to depend on other `columns` that are entirely not `primary keys`, and just restricts dependency on a `column` that is only part of the `primary key`. 

The latter, `3NF`, strictly forbids a column from depending on anything other than a `primary key`

#### Boyce-Codd (BCNF)

A table in Boyce-Codd normal form, follows all the rules of `3rd `normal form`, plus one additional rule:

- A `column` that's part of a `primary key` can not be entirely dependent on a `column` that's not part of that `primary key`.


`BCNF` is the highest possible `normalization` state a database can be in, since it strongly restricts the `primary key` parts to not depend on anything else, avoiding duplication of data the fullest, with the tradeoff of being less flexible. 

#### Rules of Thumb for Database Design

1. Every table has a `primary key`.

2. Avoid duplicate data

3. Don’t store values fully derived from other `columns` (e.g. computations). 

4. Keep `schema` simple; `normalize` first, `denormalize` only for speed.

<br>

## Joins




























