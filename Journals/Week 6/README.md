# IMAT3104 Database Management and Programming
## MongoDB Design and Migration of Relational data
### Operational details

It is a good idea to keep a copy of your solutions in a text file so you can
reuse them for future exercises, coursework and revision.

**Data Download**

A relational database concerning Video On Demand data contains seven
tables. Each table of data has been saved into a text file.

Download the zip file named VoD_relational_data.zip from Blackboard.

Unzip the file to extract the seven text files. The files are as follows:
- class.tsv
- film.tsv
- filmgenre.tsv
- director.tsv
- filmdirector.tsv
- actor.tsv
- role.tsv

### The Relational Design of the Video On Demand Database

| Table | Columns |
|-------|---------|
| Actor | actorId, firstname, lastname, dateOfBirth, nationality, biography |
| Film | filmId, title, directorFirstname, directorLastname, genre, UKReleaseDate, filename, runtime, class |
| FilmGenre | filmId, genre |
| Director | directorId, directorFirstname, directorLastname, dateOfBirth, nationality |
| FilmDirector | filmId, directorId |
| Class | classId, description |
| Role | filmId, actorId, characterName, firstAppearance, description |



The table occurrences are presented in an appendix at the end of this lab
sheet and the data in the files can be viewed in a text editor. For example, the contents of film.tsv is as follows:

The table occurrences are presented in an appendix at the end of this lab
sheet and the data in the files can be viewed in a text editor. For example, the
contents of film.tsv is as follows:

| filmId | title              | UKreleaseDate | filename              | runtime | classId |
|--------|--------------------|---------------|-----------------------|---------|---------|
| 1      | Slumdog Millionaire | 2009-01-09    | slumdogMillionaire.mpg | 120     | 15      |
| 2      | Shaft              | 2018-06-28    | shaft2019.mpg         | 111     | 15      |
| 3      | Shaft              | 2000-09-15    | shaft2000.mpg         | 99      | 18      |
| 4      | Lion               | 2017-01-20    | lion.mpg              | 118     | PG      |
| 5      | Captain Marvel     | 2019-03-08    | captainMarvel.mpg     | 123     | 12      |
| 6      | Cloud Atlas        | 2013-02-22    | cloudAtlas.mpg        | 172     | 15      |
| 7      | Shadow             | 2018-10-19    | shadow.mpg            | 176     | 18      |
| 8      | Yesterday          | 2019-06-28    | yesterday.mpg         | 116     | 12      |

Take a few moments to get familiar with the data and how the records are
related.

**Import the Data**

In MongoDB Compass, create a new database named vod2 and create new
collections with the downloaded tsv files as the names respectively. Use the
MongoDB Compass import data utility to import the data and ensure that the
Delimiter is set to TAB instead of COMMA.

**Exercise 1**

In the end, there should be seven collections of documents; one for each
imported file. For example:


Migrating the relational data into a NoSQL Design
Now the data is in MongoDB collections it can be transformed into a NoSQL
design. First, we will aggregate documents in some collections into another to
reduce the number of collections needed, known as embedding.

### Single Association Embedding

The simplest form of embedding is to embed a single value field. A film is
assigned a single Class (or Film Classification) such as PG, 12, 15 etc. We
will embed the description of the class into each associated film.

First make a duplicate of the film collection using aggregation pipeline and
name it film_c (_c is for class). This is so we can always refer back to our
original collection when we want to.

Next, run the following code to embed the descriptions from the class
collection into the corresponding documents in the film_c collection:



Open the film_c collection to see the result. You should see that a class
description field has been added to each film that corresponds with the
classId. This is an example of Single Association Embedding.


### Multiple Association Embedding using Array of Values


The next simplest form of embedding is to embed an array of values. A film
can be assigned zero, one or many film genres such as crime, comedy,
romance etc. We will embed a list of genres into each associated film.

As before, make a duplicate of the current film collection (film_c) and name it
film_cg (_cg is for class and genres).

Next, run the following code to embed each genre from the filmgenre
collection into the corresponding film in the film_cg collection:



#### Multiple Association Embedding using Array of Objects

A more complex but common form of embedding is to embed an array of
documents (or objects). A film is run by one or more directors, each with their
own set of fields such as the Indian director Loveleen Tandan and English
director Danny Boyle etc. We will embed a list of directors and their details
into each associated film.

As before, make a duplicate of the current film collection (film_cg) and name it
film_cgd (_cgd is for class, genres and directors).

Next, run the following code to embed each director and his/her details into
the corresponding film in the film_cgd collection:


Open the film_cgd collection to see the result. You should see that a directors
array has been added to each film that lists the details of directors for that
film. This is an example of Multiple Association Embedding using an Array of
Objects.


Evaluation of the Embedding design method
-----------------------------------------

Embedding is good choice when there are only a few occurrences of an
association because it will not exceed the size limit of a document. In the VoD
database example, class, genre and directors were embedded into film
because a film has only one class and only two or three genres or directors so
a film document will not grow too large as result.

Furthermore, the embedding is an excellent structure for a query to find
everything about a film including its class, genres and directors. It requires a
single and very simple query: db.film_cgd.find();

Also, deleting or updating a film or parts of a film is efficient.

Exercise 2
----------

Add a new genre to a film, say ‘Horror’ ($push) and then delete it ($pull).

Embedding is not a good choice if you want to find all classes, genres or
directors without a film. This new embedded design excluded this possibility!
In fact, the transformation lost this information. This is why the U classification
in the original class collection and the director steven spielberg in the original
director collection which were not associated with one of the films are missing
in the film_cgd collection.

It is now difficult to insert a new class, genre or director independent of a film.
It is easy to do for the original collections that you imported, but for this new
embedded design, you will need to think of a film or insert an empty film!

Exercise 3
----------

1. Insert the following class: {'R18','Adult works for licensed premises
only'} into the original class collection. This should be straight forward.
2. Insert the director {'quentin', 'tarantino', '1963-03-27', 'American'} into
the original director collection. This should be relatively easy to
understand.
3. Insert the R18 class and the director Quentin Tarantino as separate
documents into the film_cgd collection. This is relatively easy to do too,
but compared to the other documents in the film collection they look out
of place.


Multiple Association Referencing
--------------------------------

The alternative design method to embedding is referencing. A film has zero,
one or many roles played in it such as the film Captain Marvel has the
character Nick Fury being played by Samuel L. Jackson. We will reference
roles from their film.

Make a duplicate of the current film collection (film_cgd) and name it film_cgdr
(_cgdr is for class, genres, directors and roles).

We also will make changes to the role collection so make a duplicate of the
role collection and name it role_f (_f is for film).

Next, run the following code to reference each role from its corresponding film
in the film_cgdr collection. Note that the object id is used to reference roles
instead of the composite key {filmId, actorId, characterName} and the filmId
reference is removed from the role_f collection because the film now
references its roles instead.


Open the film_cgrd and role_f collections to see the results. In film_cgrd, you
should see that a roles array has been added to each film that lists the object
ids of the roles for that film and in role_f the filmId reference has been
removed. This is an example of Multiple Association Referencing.


Evaluation of Multiple Association Referencing
----------------------------------------------

Multiple Association Referencing is good when there are hundreds of
occurrences of an association because it will not exceed the size limit of a
document which it might if all of the data it is referencing was embedded. A
film would not normally have a cast list of more than one hundred roles played
by actors so each film references its roles.
This is an excellent structure to keep roles independent of films. Roles can be
queried and inserted without any regard for their films.
However, the independence that referencing provides can prove to be a
disadvantage in that both collections will need to be queried to retrieve
associated information.

Exercise 4
----------

Find the details of roles played in the film uniquely identified as filmID 1. This
will require the two collections to be “joined” or queried separately.
Another problem is that the references must be maintained. If the identifier of
a role was updated in its role document then it would need to be updated in
the film that referenced it, and similarly, if a role document was deleted then
any reference to it must also be deleted. These operations can be automated
in relational databases using the in-built mechanism of referential integrity, but
MongoDB does not have these features so the programmer must implement
them.

Exercise 5
----------

Delete the role whose character name is "Nick Fury" and any reference to it in
the film collection, then reinsert it and any references.

Single Association Referencing
------------------------------

The other form of referencing is simply to reference a single document in
another collection. Each role is played by exactly one actor. For example, the
character Saroo Brierley in the film Lion is played by Dev Patel etc. Each role
needs to reference the actor that plays that role.
This type of referencing is closest to the relational representation where a
foreign key references it primary key. In fact, there is nothing to do for the
VoD collections because they were derived from a relational database design.

Evaluation of the Referencing design method
-------------------------------------------

Single Association Referencing is good when there are thousands or millions
of instances of an association so as to limit the size of documents. For
example, if there was a concern that actors could play thousands of roles in
their lifetime then an actor could play in enough roles to overflow the 16 MB
document limit in MongoDB even if only the role ids were referenced.
Therefore, a solution is for each role to reference its actor.

Generally, referencing between different collections allows the collections to
be independent. In the last example, actors are independent of roles. Actors
can be queried and inserted without any impact on roles. However, finding
details about both actors and their roles is more complicated because both
collections need to be queried, and the actorId reference needs to be kept up
to date across collections if it changed.

Exercise 5
----------

1. Find the date of birth of the actress "Halle Berry". This should be easy.

2. Insert the details of Ziyi Zhang into the actor collection. She is an actress
and model, born on the 9th February 1979 in Beijing, China. This insert should
be straight forward.

3. Find the details of actors playing the role of the character "Minn-Erva". This
will require the two collections to be “joined” or queried separately.

4. Change the actorId of "Halle Berry" to number 10 and ensure that it is
consistent across the actor and role collections. Then change it back to its
original number. This will require several operations across several
collections.