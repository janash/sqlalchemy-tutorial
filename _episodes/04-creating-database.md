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

Create a database

~~~
"""
Make and manipulate the database
"""

import os

from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

from models import Base, Paper, Journal, Project

if os.path.isfile('my_papers.db'):
    os.remove('my_papers.db')
    
engine = create_engine('sqlite:///my_papers.db')
Session = sessionmaker(bind=engine)
session = Session()

Base.metadata.create_all(engine)

session.close()
~~~

Show in DB Browser for SQLite.

Show output from `echo=True` on creating engine to show that it is executing SQL. 

- Demonstrate adding some journals to the database.

~~~
jchem_phys = Journal(name='J. Chem. Phys', publisher='AIP')
jcim = Journal(name='J. Chem. Inf. Model', publisher='ACS')

session.add(jchem_phys)
session.add(jcim)

session.commit()
~~~
{: .language-python}

{% include links.md %}

