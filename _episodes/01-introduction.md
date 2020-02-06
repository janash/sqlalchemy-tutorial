---
title: "Introduction to Databases"
teaching: 0
exercises: 0
questions:
- "What is a database?"
- "Why/when would I use a database?"
- "What are the different types of databases?"
objectives:
- "Be able to explain what a database is."
- "Be able to name types of databases and basic properties."
keypoints:
- "A database is a collection of organized data"
- "Database Management Systems (DBMS) are used to provide access to databases."
- "Databases can be grouped into relational databases and nonrelational databases depending on structure."
---

## What is a database?

At the broadest definition, a database is a collection of organized data. Most often, when we hear the word 'databases' we think of electronic databases. Electronic databases are accessed using database management systems. Database management systems are powerful software tools which allow easy storage, access, and manipulation of data.

Databases and database management systems provide many benefits including increased data security, and ability for many people to interact with the data at once. Other advantages are that you can easily query data, and store very large datasets.

## Goal of this workshop
In this workshop, we will be working with Python to create a database which we can use to manage references. When working on an academic project or writing a paper, we often have many articles or works we would like to keep track of an cite.

Consider the following citation for a paper about The Molecular Sciences Software Institute

> Krylov, A.; Windus, T. L.; Barnes, T.; Marin-Rimoldi, E.; Nash, J. A.; Pritchard, B.; Smith, D. G. A.; Altarawy, D.; Saxe, P.; Clementi, C.; Crawford, T. D.; Harrison, R. J.; Jha, S.; Pande, V. S.; Head-Gordon, T. Perspective: Computational Chemistry Software and Its Advancement as Illustrated through Three Grand Challenge Cases for Molecular Science. The Journal of Chemical Physics 2018, 149 (18), 180901.

This citation has a lot of data associated with it. There are a number of authors, a title, the journal, publication year, DOI, etc. Each citation has this information, and you likely need to track hundreds (thousands??) of these throughout your research career. We will make a database which we can use to manage all of our references. We would like to be able to add and search data on journals, and authors and associate citations with projects.

The first thing we must do before we make our database is decide the type of database we would like to have. The main choice we will be making is whether we want to have a **relational** database or a **nonrelational** database.

## Database types - relational vs nonrelational

Database structure differs depending on the type of data in the database. If your data has many relations that you would like to be able to query, you will use a relational database. A relational database is made up of several tables which specific schema.  These tables are connected by relationships between them (more on this later).

A non-relational database is does not have tables or relationships. A very popular non-relational database software is [mongoDB](https://www.mongodb.com/). In mongoDB, you have collections of data with associated documents. The structure of the document can vary for each entry. Because there is no schema,non relational databases are more flexible than relational databases. However, because there is no schema or relations, data is often duplicated within the database.

## Choosing a database type
For our selected application, it may not be immediately clear which type of database we should use. Consider the following about the data we will be storing

- Citations tend to have similar information (well defined structure).
- We would like to be able to group our citations by project and journal.
- The data we are working with is not very large.

The choice is not obvious for this example, but we are going to choose to implement our reference manager using a relational database. This will allow us to define a schema and relationships in our data (maybe we will implement the same thing using mongoDB in the future? stay tuned). We will learn more about how to structure tables and schemas in the next lesson. 

{% include links.md %}

