###   
# Relational database with SQLite and SQLAlechmy

## Goals

* Create a relational database for a pipeline, with Shots, Assets, Tasks and Casting (Shot/Asset linking)
* Access the database in python, using SQLAlchemy
* Ultimatively: use this database as an abstraction and cache layer to Shotgrid and/or kitsu.

###
## Reading material:

https://realpython.com/python-sqlite-sqlalchemy/

###
## Setup SQLite

### Install

Download here:
https://www.sqlite.org/download.html  
For Windows, download the **DLL** and the **tools** zip.


### Rez

You can also use the rez package I created   
(here: `\\marvin\WIP\STUDENTS\TD4\PIPELINE\rez\packages\lib\sqlite`)

`rez env sqlite`


### Setting up the Database

To test if it works, and start working, we create a database.
https://www.tutorialspoint.com/sqlite/sqlite_create_database.htm

```
rez env sqlite
sqlite3 D:\<PATH>\base.db
```

**base.db** will be the name of the database file.
SQLite uses one file for the database.

###
## Set up a Database GUI

Next step, is to install a UI, to be able to handle the database visually.

There are many options.
Some are SQLite specific (https://sqlitebrowser.org/), and others handle multiple RDBMS ("Relational DataBase Management Systems"), like PgSQL, MySQL, MS SQL, Oracle.

Amongst the tools for multiple database systems (RDBMS), some are webbased (https://www.adminer.org/ in PHP), some are desktop apps.

Hani recommands DBeaver, https://dbeaver.io/.

For Windows, I suggest to use the https://dbeaver.io/download/ ZIP version, which we can put in a rez package.

The rez package should be local, because the network install is very slow.
You can create your package in `D:\rez\dev_packages\dbeaver`

With the zip extracted into a subfolder called dbeaver, and a package.py

```
name="dbeaver"
def commands():
    env.PATH.append("{root}/dbeaver")
```
###


