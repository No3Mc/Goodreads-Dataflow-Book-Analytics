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

Appendix – Relational data
--------------------------

Class
-----

| classId | description                                                 |
| ------- | ----------------------------------------------------------- |
| U       | Universal: Suitable for all                                 |
| PG      | Parental Guidance: General viewing but some scenes may be    |
|         | unsuitable for young children                                |
| 12      | Suitable only for persons of 12 years or over                |
| 15      | Suitable only for persons of 15 years or over                |
| 18      | Suitable only for persons of 18 years or over                |



Film
----

| filmId | Title               | UKreleaseDate | filename              | runtime | classId |
| ------ | ------------------ | ------------- | --------------------- | ------- | ------- |
| 1      | Slumdog Millionaire | 2009-01-09    | slumdogMillionaire.mpg | 120     | 15      |
| 2      | Shaft               | 2018-06-28    | shaft2019.mpg         | 111     | 15      |
| 3      | Shaft               | 2000-09-15    | shaft2000.mpg         | 99      | 18      |
| 4      | Lion                | 2017-01-20    | lion.mpg              | 118     | PG      |
| 5      | Captain Marvel      | 2019-03-08    | captainMarvel.mpg     | 123     | 12      |
| 6      | Cloud Atlas         | 2013-02-22    | cloudAtlas.mpg        | 172     | 15      |
| 7      | Shadow              | 2018-10-19    | shadow.mpg            | 176     | 18      |
| 8      | Yesterday           | 2019-06-28    | yesterday.mpg         | 116     | 12      |

FilmGenre
---------

| filmId | genre     |
| ------ | --------- |
| 1      | Drama     |
| 1      | Romance   |
| 2      | Action    |
| 2      | Comedy    |
| 2      | Crime     |
| 3      | Action    |
| 3      | Crime     |
| 3      | Thriller  |
| 4      | Biography |
| 4      | Drama     |
| 5      | Action    |
| 5      | Adventure |
| 5      | Sci-Fi    |
| 6      | Action    |
| 6      | Drama     |
| 6      | Mystery   |
| 7      | Action    |
| 7      | Drama     |
| 7      | War       |
| 8      | Comedy    |
| 8      | Fantasy   |
| 8      | Music     |


Director
--------

| directorId | directorFirstname | directorLastname | dateOfBirth  | nationality |
| ---------- | ----------------- | ---------------- | ------------ | ----------- |
| 1          | danny             | boyle            | 1956-10-20   | English     |
| 2          | loveleen          | tandan           | N/A          | Indian      |
| 3          | tim               | story            | 1970-03-13   | American    |
| 4          | john              | singleton        | 1968-01-06   | American    |
| 5          | garth             | davis            | N/A          | Australian  |
| 6          | anna              | boden            | 1979-10-20   | American    |
| 7          | ryan              | fleck            | 1976-09-20   | American    |
| 8          | steven            | spielberg        | 1946-12-18   | American    |
| 9          | tom               | tykwer           | 1965-05-23   | German      |
| 10         | lana              | wachowski        | 1965-06-21   | American    |
| 11         | lilly             | wachowski        | 1967-12-29   | American    |
| 12         | yimon             | zhang            | 1951-11-14   | Chinese     |


FilmDirector
------------


| filmId | directorId |
| ------ | ---------- |
| 1      | 1          |
| 1      | 2          |
| 2      | 3          |
| 3      | 4          |
| 4      | 5          |
| 5      | 6          |
| 5      | 7          |
| 6      | 9          |
| 6      | 10         |
| 6      | 11         |
| 7      | 12         |
| 8      | 1          |


Actor
-----

| actorId | firstname    | lastname | dateOfBirth | nationality | biography                                                               |
| ------- | ------------ | -------- | ----------- | ----------- | ----------------------------------------------------------------------- |
| 1       | dev          | patel    | 1990-04-23  | English     | ... born in Harrow, London, to Anita, a caregiver, and Raj Patel, who works in IT. His parents, originally from Nairobi, Kenya, are both of Gujarati Indian descent. His first role was in the UK TV series Skins (2007) ....  |
| 2       | freida       | pinto    | 1984-10-18  | Indian      | ... born in Bombay (now Mumbai), Maharashtra, India, to Sylvia, a school principal, and Frederick Pinto, a senior bank branch manager. She traversed the modeling circuit in Mumbai for two years before gaining her big break in a movie ...                                                                                  |
| 3       | samuel l.    | jackson  | 1948-12-21  | American    | ... born in Washington, D.C., to Elizabeth (Montgomery) and Roy Henry Jackson. He was raised by his mother, a factory worker, and his grandparents. At Morehouse College, Jackson was active in the black student movement ...                                                                               |
| 4       | fawad        | khan     | 1981-11-29  | Pakistani   | ... born in Karachi to a family originally based in Lahore. He spent earlier years of his life abroad. Khan, as his father's job required, lived in Athens, Dubai, Saudi Arabia and Riyadh and then went to Manchester during the Gulf War, moving back to Lahore ...                                                                                  |
| 5       | gemma        | chan     | 1982-11-29  | English     | ... born at Guy's Hospital in London, England. Her father grew up in Hong Kong and was an engineer. Her mother, a pharmacist at Guy's Hospital, emigrated from China via Hong Kong with Chan's maternal grandparents and younger sister, growing up in Greenock, Scotland ...                                                                                  |
| 6       | halle        | berry    | 1966-08-14  | American    | ... born in Cleveland, Ohio. Her father, Jerome Jesse Berry, was African-American, and worked as a hospital attendant. Her mother, Judith Ann (Hawkins), who is Caucasian, has English and German ancestry, and is a retired psychiatric nurse ...                                                                                    |
| 7       | wu           | lei      | 1999-12-26  | Chinese     | ... born in Shanghai, China. He is an actor, known for Nirvana in Fire (2015), Whirlwind Girl 2 (2016) and The Shanghai Job (2017) ...                                                                                               |


Role
----

| filmId | actorId | characterName | firstAppearance | description                                                                                                                                                |
| ------ | ------- | ------------- | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | 1       | Older Jamal   | 81              | A Mumbai teenager reflects on his life after being accused of cheating on the Indian version of "Who Wants to be a Millionaire?"                                |
| 1      | 2       | Older Latika  | 396             | She was raised in the slums of Mumbai. Orphaned at a young age, she managed to survive with her friends Jamal and his older brother Salim and formed a close relationship. |
| 2      | 3       | John Shaft    | 55              | A cyber security expert with a degree from MIT, enlists his family's help to uncover the truth behind his best friend's untimely death.                          |
| 3      | 3       | John Shaft    | 72              | A New York City police detective goes on a personal mission to make sure the son of a real estate tycoon is brought to justice after a racially-motivated murder. |
| 4      | 1       | Saroo Brierley | 378             | A five-year-old Indian boy gets lost on the streets of Calcutta, thousands of kilometers from home. He survives many challenges before being adopted by a couple in Australia. 25 years later, he sets out to find his lost family. |
| 5      | 3       | Nick Fury     | 482             | A veteran S.H.I.E.L.D. operative, Nick is one of the greatest super spies in the world. When Nick crosses paths with Captain Marvel, they become Earth's only hope of stopping a Skrull invasion. |
| 5      | 5       | Minn-Erva     | N/A             | A Kree sniper and a member of Starforce, Minn-Erva, and the Starforce eventually locate and capture the defected Kree warrior Captain Marvel and her friends. As Minn-Erva and the Kree prepares to kill them and the remaining Skrull refugees, Captain Marvel frees herself, defeating the Starforce in combat, allowing enough time for Rambeau to pilot her ship. Minn-Erva finally attemptes to eliminate Captain Marvel's new allies, only for Rambeau to shoot and destroy her ship, killing her in the process. |
| 6      | 6       | Luisa Rey     | 2880            | A journalist in 1973 San Francisco, meets Rufus Sixsmith in an elevator and he notices that she has a star shaped birth mark just like others in the film. Later she finds dead body of Rufus in his hotel room and finds out about the Cloud Atlas Sextet. |
| 6      | 6       | Meronym       | 8970            | A prescient who visits the village of a man called Zachry in the year 2311. A prescient is a human who is able to use advanced technology. She is able to heal the niece of Zachry who gets sick from radiation with a tumor growing on her foot. He helps her climb to a trans way station at the top of a mountain where she broadcasts an emergency message to the human colonies in space asking them to rescue them. Ultimately, she and Zachry and his niece go to the outer space colonies together. |
| 7      | 7       | Yang Ping     | 176             | The young prince of General Yang is set to marry the sister of the king of Pei in a diplomatic solution to control the warring region.                               |
