---
layout: post
title:      "Understanding Join Tables and Relational Databases"
date:       2020-04-29 17:31:50 -0400
permalink:  understanding_join_tables_and_relational_databases
---


Boy, do I love SQL. After 8 grueling weeks of Procedural and Object Oriented Ruby (which was equivalent to quite literally learning a new langauge for me), I finally feel like I am cruising through SQL and databases. I, for one, love data. I've always enjoyed working in spreadsheets, collecting data, analyzing data, and using it to solve business problems (no, I am not a data scientist). 

I think part of the reason I enjoy learning about SQL and relational databases is that it is a much more visual process than building methods in OO Ruby. Not to mention, the language is more literal (e.g. "CREATE TABLE," "INSERT INTO," "SELECT * FROM" etc...). Relational concepts are also much easier for me to understand when I can create a visual structure prior to building a database.

When building databases, the "belongs_to/has_many" relationship is established with the use of foreign keys. When building a table, the child entity (i.e. the one that belongs to another entity) has a foreign key column. Here's an example:

![](https://i.imgur.com/KZ09AFP.jpg)

As you can see above, Cujo and Charlie both have an owner_id of 1, and therefore belong to Beth. Luna has an owner_id of 2, belonging to Steven. And finally, Dakota has an owner_id of 3 and belongs to Ruth.

So how do we go about storing data consisting of many-to-many relationships? This is where things get fun, and a little complex (IMHO). Enter: *join tables*. Join tables allow you to store data with many-to-many associations by having a field of common data from multiple tables (typically foreign keys). A great example of this is one of the labs in our Learn.co curriculum, SQL Library Lab. In this lab, I had to create a table for the following: Characters, Books, Series, Authors, and Sub-Genres. In order to create complex SQL queries and create proper associations, I had to create a join table. Before getting started, I went ahead and physically illustrated the tables I was building and their relationships with one another.

![](https://imgur.com/7698X3H)

As you can see, I built the join table to connect both books and characters, as books have many characters and characters have many books. The join table is called character_books and needs two foreign key columns, like so:

```
CREATE TABLE character_books (
id INTEGER PRIMARY KEY,
book_id INTEGER,
character_id INTEGER);
```

Once all of the tables were built, I simply had to write queries that would complete the tests. Here's an example of one a query involving the join table:

```
def select_character_names_and_number_of_books_they_are_in
     "SELECT characters.name, COUNT(*) as book_count
     FROM characters
     JOIN character_books 
     ON character_books.character_id = characters.id 
     GROUP BY characters.name 
     ORDER BY book_count DESC"
end
```

Join tables make these complex relationships and complicated database queries possible! The biggest takeaway of this week for me is this: *when it doubt, draw it out.*


