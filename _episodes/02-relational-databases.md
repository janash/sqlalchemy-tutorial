---
title: "Deciding Database Structure"
teaching: 0
exercises: 0
questions:
- "What is a relational database?"
- "How is data organized in a relational database?"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

## Database structure

In order to create our SQL database, our first step will be to decide the structure.

### Tables
The Table is the basic unit of storage in a relational database. A table is made up of columns which define the data being stored in the table, and rows (records) which are entries in the table.

Before deciding specific tables of our database, let's consider some of our goals

- We want to store citation information for papers.
- We would also like to retrieve papers by journal or author.
- We want to be able to group papers by research project.

Think about how we might group information about journals into a spreadsheet. Consider what information we might have about our journals.
- journal name
- publisher
- associated articles.

Consider the 'associated articles' entry. This cell in a spreadsheet would have multiple values. In a relational database, this would be a violation of **first normal form** which states that each attribute should be atomic (have indivisible values). 

Walk through wikipedia example basically - (https://en.wikipedia.org/wiki/First_normal_form)

If we want to associate relations with this table in our database, we use two tables.

This would result in the following data structure

Journal Table
- journal name
- publisher

Journal-Paper Table
- article
- journal name

Where `journal name` is shared between the tables. In the Journal - Paper Table, we can have a journal name repeated several times.

The journal paper table is what is referred to as an associative table.

Exercise 
Work on structure of other tables. We want to store information about each paper and project.

Paper Table
- DOI
- title
- publication year
- journal name

Author Table
- Author Name
- Author affiliation

Project Table
- Project name
- Project description

## primary keys
special column (or columns) which are unique identifiers for the table records.

Association tables

- Journal-Paper

- Author-Paper

- Project-Paper










{% include links.md %}

