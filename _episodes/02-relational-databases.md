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
        <td>T. Daniel Crawford</td>
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

Now, there is only one entry for each article in the article table, and a separate entry matching each author with an article.  When we choose to express information in this way, we assume that names are unique (as in, no two authors can have the same name). This may not be true, so you may want to implement some other identifier for authors such as an ID number. No matter if we use names or IDs, we will need another name that lists all unique authors. If we do this, we will replace the author table with something like this

<table style="width:50%">
    <tr>
        <th>Article DOI</th>
        <th>Author ID</th>
    </tr>
    <tr>
        <td>'10.1063/1.5052551'</td>
        <td>1</td>
    </tr>
    <tr>
        <td>'10.1063/1.5052551'</td>
        <td>2</td>
    </tr>
    <tr>
        <td>'10.1063/1.5052551'</td>
        <td>3</td>
    </tr>
    <tr>
        <td>'10.1063/1.5052551'</td>
        <td>4</td>
    </tr>
    <tr>
        <td>'10.1063/1.2137323'</td>
        <td>5</td>
    </tr>
    <tr>
        <td>'10.1063/1.2137323'</td>
        <td>2</td>
    </tr>
</table>

Since names can sometimes repeat, we probably want to use some other identifier for the author like an ID.

## Article - Author Table


<table style="width:50%">
    <tr>
        <th>Author ID</th>
        <th>Author Name</th>
    </tr>
    <tr>
        <td>1</td>
        <td>Anna Krylov</td>
    </tr>
    <tr>
        <td>2</td>
        <td>T. Daniel Crawford</td>
    </tr>
    <tr>
        <td>3</td>
        <td>Theresa Windus</td>
    </tr>
    <tr>
        <td>4</td>
        <td>Taylor Barnes</td>
    </tr>
</table>


### Identifying unique entries - Primary Keys

We know have three tables which express information about our papers and authors. In a relational database, having unique identifiers is important for records in tables. Consider the tables we currently have.

We have a `papers` table which tells us a unique identifier for each paper (the DOI). The DOI should never be repeated. If a new paper is added, it will never have the same DOI as a previously added paper (otherwise, it will be that paper! The DOI is a unique identifier)

We also have a unique identifier (Author ID) in the `authors` table. 

The examples of the `DOI` and `Author ID` are known as `primary keys` in the table. A primary key **uniquely specifies** a record in a table. Each table MUST have a primary key.

Primary keys may be single columns, or they may be multiple columns. For `authors` and `papers`, we have single column primary keys. For our `article-author` table, you will see that neither of the columns has unique values. Both authors and papers can be repeated. However, we would never expect the combination of the two columns to repeat. In this case, our primary key is both columns. We'll see more on this later.

## Relationships
Central to the idea of relational databases are...relationships! You can already see from inspection how these tables are related. You would be able to figure out authors for a paper by tracing relationships through our three tables. Relational databases have specific relationship patterns, so let's discuss these.

### Many to Many relationships
The article-author table we've created is what is known as an associative table. Consider the type of relationship between papers and authors. One paper can have many authors, and one authors can have many papers (consider that T. Daniel Crawford is an author on two of our papers). This is called a *many to many* relationship. When we have these in a database, we will have an associative table. In addition to the two tables outlined, we will also have an author table (where each author is listed by ID) which the `article-author` table will index into. 

### One to Many and Many to One relationships
Another type of relationship is the *one to many* relationship. An example of this is the relationship between journals and papers. Papers belong to only one journal, but a journal can have many papers associated with it. We do not need an associative table for this type of relationship because the paper table can reference the journal ID directly in a column on the table (since there is only one value per paper).

### Foreign Keys

The columns in the `article-author` table are also examples of **foreign keys**. Foreign keys reference values in other tables. For example, the `Article DOI` column in the `article-author` table references the `DOI` column in the  `article` table. Foreign keys are constraints, we should never have an article DOI appear in the `article-author` table which does not have an entry in the `article` table. 

The `author ID` column in the `article-author` table is a foreign key which references the `author ID` column in the `author` table. Don't worry if this is all a lot to remember right now! We will work on applying these ideas later.

## Deciding Table Structure

By going through the information we would like to store and enforcing atomicity and reducing redundancy, we can come up with a table structure.

We will now limit redundancy and make the values atomic. We will need to create new tables in some cases in order to associate tables together.

> ## Exercise 
> Take a few minutes to think about the information we have and how we might organize them into tables.
> 
> Remember that we already have tables for `articles`, `authors` and an association table for matching articles and authors.
>
> We now want to add tables for journals, and for grouping articles together by the project we are working on. For journals, we want to store the journal name and a description of the journal. For projects, we also want a project name and a description.
>
> Label the primary keys and any foreign keys for each table.
>
>> ## Solution
>> We start with the following tables.
>> ### Papers Table
>> Columns:
>> - DOI (primary key)
>> - title
>> - publication year
>> - journal (foreign key to `name` column in `Journals` table.)
>> 
>> ### Authors Table
>> Columns:
>> - ID (primary)
>> - name
>>
>> ### Paper - Authors Table
>> Columns: 
>> - Article DOI (foreign key to `DOI` column in `papers` table, primary key)
>> - Author ID (foreign key to `author ID` column in `Authors` table, primary key)
>>
>> ### Journals Table
>> Columns:
>> - name (primary key)
>> - description
>>
>> ### Project Table
>> Columns:
>> - name (primary key)
>> - description
>>
>> Since a paper may belong to many projects, and a project will have many papers associated with it, this is a `many-to-many` relationship. We will also need an association table for matching projects and papers.
>> 
>> ### Paper - Project Table
>> Columns:
>> - Paper DOI (foreign key to `DOI` in `papers` table, primary key)
>> - Project name (foreign key to `name` in `projects` table, primary key)
> {: .solution}
{: .challenge}


{% include links.md %}

