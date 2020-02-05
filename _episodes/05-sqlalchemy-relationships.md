---
title: "Relationships"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

Linking tables together

## One to Many Relationships

A one to many relationship places a foreign key on the child table referencing the parent. relationship() is then specified on the parent, as referencing a collection of items represented by the child:

- Journal is the parent
- Paper is the child.

~~~
class Journal(Base):
    __tablename__ = 'journals'

    name = Column(String, primary_key=True)
    publisher = Column(String, nullable=True)
    papers = relationship('Paper')


class Paper(Base):
    __tablename__ = 'papers'

    DOI = Column(String, primary_key=True)
    paper_title = Column(String, nullable=False)
    publication_year = Column(Integer, nullable=False)

    journal_name = Column(String, ForeignKey('journals.name'))


    def __str__(self):
        return F'Papers(DOI={self.DOI}, paper_title={self.paper_title})'
~~~

Foreign Keys

A FOREIGN KEY is a key used to link two tables together.

A FOREIGN KEY is a field (or collection of fields) in one table that refers to the PRIMARY KEY in another table.

A foreign key in SQL is a table-level construct that constrains one or more columns in that table to only allow values that are present in a different set of columns, typically but not always located on a different table. 

- Add ForeignKey to imports (from sqlalchemy)

Test it out:

~~~
molssi_paper = Paper(DOI='10.1063/1.5052551', paper_title='Perspective: Computational chemistry software and its advancement as illustrated through three grand challenge cases for molecular science', publication_year=2018)

# Add another paper
bse_paper = Paper(DOI='10.1021/acs.jcim.9b00725', paper_title='New Basis Set Exchange: An Open, Up-to-Date Resource for the Molecular Sciences Community', publication_year=2019)

# Add another paper for J Chem Phys
crawford_paper = Paper(DOI='10.1063/1.2137323', paper_title='Sources of error in electronic structure calculations on small chemical systems', publication_year=2006)

jchem_phys = Journal(name='J. Chem. Phys', publisher='AIP', papers=[crawford_paper, molssi_paper])
#jcim = Journal(name='J. Chem. Inf. Model', publisher='ACS')

session.add(jchem_phys)
~~~
{: .language-python}

Demonstrate this does not work the other way

~~~
molssi_paper = Paper(DOI='10.1063/1.5052551', paper_title='Perspective: Computational chemistry software and its advancement as illustrated through three grand challenge cases for molecular science', publication_year=2018, journal_name='J Chem Phys')

session.add(molssi_paper)

session.commit()
~~~
{: .language-python}

Journal table is not filled out.

### Many to One

Add backref to papers in journal table

~~~
papers = relationship('Paper', backref='journal')
~~~
{: .language-python}

** Note that 'journal' does not already exist as a column. This associates a journal object with paper_object.journal. paper_object.journal_name has the name of that journal.

Now you can pass a journal instance to Paper to achieve the linking.

~~~

molssi_paper = Paper(DOI='10.1063/1.5052551', paper_title='Perspective: Computational chemistry software and its advancement as illustrated through three grand challenge cases for molecular science', publication_year=2018, journal=jchem_phys)
~~~
{: .language-python}

If you keep using `journal_name` it doesn't fill in the journal table as well.

## Many to Many Relationships

~~~
association_table = Table('project_papers', Base.metadata, 
    Column('project_name', String, ForeignKey('projects.name')),
    Column('paper_doi', String, ForeignKey('papers.DOI'))
)

class Project(Base):
    __tablename__ = 'projects'

    id = Column(Integer, primary_key=True)
    name = Column(String, unique=True, nullable=False)
    description = Column(String, nullable=True)
    papers = relationship("Paper", secondary=association_table, backref="projects")
~~~
{: .language-python}

~~~

# Adding a project
molssi_project = Project(name="MolSSI Fellowship", description="Papers associated with MolSSI",
        papers=[molssi_paper, bse_paper]
)
session.add(molssi_project)
session.commit()

# To add another paper
#from_db = session.query(Project).filter(Project.name=='MolSSI Fellowship').one()
#from_db.papers.append(crawford_paper)

#session.add(from_db)
#session.commit()

session.close()
~~~
{: .language-python}

{% include links.md %}

