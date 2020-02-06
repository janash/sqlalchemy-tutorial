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
        <tr>
        <td>'10.1021/acs.jcim.9b00725'</td>
        <td>'New Basis Set Exchange: An Open, Up-to-Date Resource for the Molecular Sciences Community'</td>
        <td>J. Chem. Inf. Model</td>
        <td>2019</td>
        <td>'Benjamin P. Pritchard, Doaa Altarawy, Brett Didier, Tara D. Gibson, Theresa L. Windus'</td>
        <td></td>
    </tr>
</table>

To put this into a database, we will want to make some changes to this structure. Imagine that we have hundreds of articles in our spreadsheet. What if we wanted to find all papers with one particular author? This would be very hard to find.

In relational databases, values which can be searched should be **atomic** (meaning indivisible). The author row lists multiple values, meaning that it violates [first normal form](https://en.wikipedia.org/wiki/First_normal_form).

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

We have now fixed the problem of repeating authors, but we now repeat all other information several times. If there was a change to the title of a paper, for example, we would have to update this in all the associated rows. Our goal is to reduce this repetition.

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
    <tr>
        <td>'10.1021/acs.jcim.9b00725'</td>
        <td>'New Basis Set Exchange: An Open, Up-to-Date Resource for the Molecular Sciences Community'</td>
        <td>J. Chem. Inf. Model</td>
        <td>2019</td>
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
    <tr>
        <td>'10.1063/1.2137323'</td>
        <td>T. Daniel Crawford</td>
    </tr>
</table>

Now, there is only one entry for each article in the article table, and a separate entry matching each author with an article. 

### Primary Keys

We see that in this table, both article and author are repeated. Assuming that all authors can be uniquely identified by their name, we would also want to add a table where author information is presented. Since names can sometimes repeat, we probably want to use some other identifier for the author like an ID.

## Relationships

### Many to Many relationships
The article-author table we've created is what is known as an associative table. Consider the type of relationship between papers and authors. One paper can have many authors, and one authors can have many papers (consider that T. Daniel Crawford is an author on two of our papers). This is called a *many to many* relationship. When we have these in a database, we will have an associative table. In addition to the two tables outlined, we will also have an author table (where each author is listed by ID) which the `article-author` table will index into. 

### One to Many and Many to One relationships
Another type of relationship is the one to many relationship. An example of this is the relationship between journals and papers. Papers belong to one journal, but a journal can have many papers associated with it. We do not need an associative table for this type of relationship because the paper table can reference the journal ID directly in a column on the table (since there is only one value per paper).

### Foreign Keys

## Deciding Table Structure

By going through the information we would like to store and enforcing atomicity and reducing redundancy, we can come up with a table structure.

We will now limit redundancy and make the values atomic. We will need to create new tables in some cases in order to associate tables together.

> ## Exercise 
> Take 10 minutes to think about the information we have and how we might organize them into tables. Work on structure of other tables.
>
> We will want tables for papers, journals, authors, and author institutions.
>
>> ## Solution
>> We start with the following tables.
>> ### Papers Table
>> Columns:
>> - DOI
>> - title
>> - publication year
>> - journal
>> 
>> ### Journals Table
>> Columns:
>> - name
>> - description
>>
>> ### Authors Table
>> Columns:
>> - ID
>> - name
>> - institution ID
>> 
>> ### Institution
>> Columns:
>> - Institution ID
>> - name
>> 
>> We will also need an association table 
> {: .solution}
{: .challenge}

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

