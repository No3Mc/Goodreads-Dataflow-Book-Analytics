# IMAT3104 Database Management and Programming
## MongoDB Design and Migration of Relational data
### Operational details

It is a good idea to keep a copy of your solutions in a text file so you can
reuse them for future exercises, coursework and revision.

#### Data Download
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

#### The Relational Design of the Video On Demand Database

| Table | Columns |
|-------|---------|
| Actor | actorId, firstname, lastname, dateOfBirth, nationality, biography |
| Film | filmId, title, directorFirstname, directorLastname, genre, UKReleaseDate, filename, runtime, class |
| FilmGenre | filmId, genre |
| Director | directorId, directorFirstname, directorLastname, dateOfBirth, nationality |
| FilmDirector | filmId, directorId |
| Class | classId, description |
| Role | filmId, actorId, characterName, firstAppearance, description |


