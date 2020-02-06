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

In the previous lesson, we mentioned that relational databases are structured and have a specific schema. In order to create our relational database, our first step will be to decide the structure. Relational databases have specific rules which should be used in the database design. The goal of relational databases is to reduce repetition of information and dependencies between rows, this is called normalization.

### Tables
The Table is the basic unit of storage in a relational database. A table is made up of columns which define the data being stored in the table, and rows (also called records) which are entries in the table.

First, consider what information about some of our papers would look like if we just added it to a spreadsheet.

<table style="width:100%">
    <tr>
        <th>DOI</th>
        <th>Title</th>
        <th>Journal</th>
        <th>Publication Year</th>
        <th>Authors</th>
    </tr>
    <tr>
        <td>'10.1063/1.5052551'</td>
        <td>'Perspective: Computational chemistry software and its advancement as illustrated through three grand challenge cases for molecular science'</td>
        <td>J. Chem. Phys.</td>
        <td>2018</td>
        <td>Anna Krylov, Theresa L. Windus, Taylor Barnes, Eliseo Marin-Rimoldi, Jessica A. Nash, Benjamin Pritchard, Daniel G.A. Smith, Doaa Altarawy, Paul Saxe, Cecilia Clementi, T. Daniel Crawford, Robert J. Harrison, Shantenu Jha, Vijay S. Pande, Teresa Head-Gordon</td>
    </tr>
    <tr>
        <td>'10.1063/1.5052551'</td>
        <td>'Sources of error in electronic structure calculations on small chemical systems'</td>
        <td>J. Chem. Phys.</td>
        <td>2006</td>
        <td>David Feller, Kirk A. Peterson, T. Daniel Crawford</td>
    </tr>
</table>

To put this into a database, we will want to make some changes to this structure. Imagine that we have hundreds of articles in our spreadsheet. What if we wanted to find all papers with one particular author? This would be very hard to find.

In relational databases, values which can be searched should be **atomic** (meaning indivisible). The author row violates this, meaning that it violates [first normal form](https://en.wikipedia.org/wiki/First_normal_form).

One solution would be to add several more columns to our spreadsheet (Author1, Author2, Author3). However, this would results in a varying number of columns for each paper. 

Another solution would be to have a separate row for each author

<table style="width:100%">
    <tr>
        <th>DOI</th>
        <th>Title</th>
        <th>Journal</th>
        <th>Publication Year</th>
        <th>Author</th>
    </tr>
    <tr>
        <td>'10.1063/1.5052551'</td>
        <td>'Perspective: Computational chemistry software and its advancement as illustrated through three grand challenge cases for molecular science'</td>
        <td>J. Chem. Phys.</td>
        <td>2018</td>
        <td>Anna Krylov</td>
    </tr>
    <tr>
        <td>'10.1063/1.5052551'</td>
        <td>'Perspective: Computational chemistry software and its advancement as illustrated through three grand challenge cases for molecular science'</td>
        <td>J. Chem. Phys.</td>
        <td>2018</td>
        <td>Theresa L. Windus</td>
    </tr>
    <tr>
        <td>'10.1063/1.2137323'</td>
        <td>'Perspective: Computational chemistry software and its advancement as illustrated through three grand challenge cases for molecular science'</td>
        <td>J. Chem. Phys.</td>
        <td>2018</td>
        <td>Taylor Barnes</td>
    </tr>
</table>

We have now fixed the problem of repeating authors, but we now repeat all other information several times. If there was a change to the title of this paper, for example, we would have to update this in all the associated rows. Our goal is to reduce this repetition.

Ultimately, the design which minimizes repetition and enforces atomicity is adding multiple sheets or tables.

## Article Table
<table style="width:100%">
    <tr>
        <th>DOI</th>
        <th>Title</th>
        <th>Journal</th>
        <th>Publication Year</th>
    </tr>
        <tr>
        <td>'10.1063/1.5052551'</td>
        <td>'Perspective: Computational chemistry software and its advancement as illustrated through three grand challenge cases for molecular science'</td>
        <td>J. Chem. Phys.</td>
        <td>2018</td>
    </tr>
    <tr>
        <td>'10.1063/1.2137323'</td>
        <td>'Sources of error in electronic structure calculations on small chemical systems'</td>
        <td>J. Chem. Phys.</td>
        <td>2006</td>
    </tr>
</table>


## Article - Author Table
<table style="width:50%">
    <tr>
        <th>Article DOI</th>
        <th>Author</th>
    </tr>
    <tr>
        <td>'10.1063/1.5052551'</td>
        <td>Anna Krylov</td>
    </tr>
    <tr>
        <td>'10.1063/1.5052551'</td>
        <td>Theresa L. Windus</td>
    </tr>
    <tr>
        <td>'10.1063/1.5052551'</td>
        <td>Taylor Barnes</td>
    </tr>
    <tr>
        <td>'10.1063/1.2137323'</td>
        <td>David Feller</td>
    </tr>
</table>

, Kirk A. Peterson, T. Daniel Crawford

Now, there is only one entry for each article in the article table, and a separate entry matching each author with an article.


Think about how we might group information about journals into a spreadsheet. Consider what information we might have about our journals. B
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

