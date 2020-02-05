---
title: "Introduction"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

> ## Callout - Structured Query Language - SQL
> SQL (Structured Query Language) is used to communicate with relational databases.
{: .callout}

## Types of SQL Databases
- SQLite
- MySQL
- Postgres

All are SQL databases but have slightly different syntax, etc. SQLAlchemy is a python library which allows us to write Python and change out the back end we use for the database.

SQLAlchemy contains an ORM - object relational mapper - which can map Python objects to SQL tables. Although the ORM is widely used with SQLAlchemy, you can use SQL Alchemy without the ORM.

### Our first pass at our tables

In order to make our table models using the ORM, we need to use a declarative base from SQLAlchemy.
~~~
"""
Table models for SQLAlchemy database.
"""

from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()
~~~
{: .language-python}

To make a table, we create a class which inherits from this declarative base.

~~~
class Journal(Base):
    __tablename__ = 'journals'
~~~
{: .language-python}

This creates a class called journal which inherits from Base, and names the table that will be associated with this class 'journals'.

Next, we define our table columns. The variable names represent table columns, and will be attributes of the class when we create instances of this class.

Add the imports
~~~
from sqlalchemy import Column, Integer, String
~~~
{: .language-python}

~~~
class Journal(Base):
    __tablename__ = 'journals'

    name = Column(String, primary_key=True)
    publisher = Column(String, nullable=True)
~~~
{: .language-python}

Open python REPL and show that it is an object. Call help to show it has inherited from sqlalchemy base.

Define the rest of the non-associative tables.

~~~
class Paper(Base):
    __tablename__ = 'papers'

    DOI = Column(String, primary_key=True)
    paper_title = Column(String, nullable=False)
    publication_year = Column(Integer, nullable=False)
    journal_name = Column(String)

class Author(Base):
    __tablename__ = 'authors'

    name = Column(String, primary_key=True)
    affiliation = Column(String, nullable=True)

class Project(Base):
    __tablename__ = 'projects'

    name = Column(String, primary_key=True)
    description = Column(String, nullable=True)
~~~


{% include links.md %}

