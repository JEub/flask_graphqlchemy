# Flask + GraphQL + SQLAlchemy

This example project follows the docs found [here](https://docs.graphene-python.org/projects/sqlalchemy/en/latest/tutorial/).

## Modifications

There were some items that I just didn't want to use from the source example. I have listed the changes that I made here, as well as how that impacts the code overall. 

### Virtual Environment Setup

I wanted to use PostgreSQL as the backend instead of the included sqlite. 

```bash
# Assumes you are in the project folder

# create a virtual environment using venv and activate environment
python3.7 -m venv venv
source venv/bin/activate

# Adds the original python packages locally
pip install --upgrade pip
pip install flask flask-graphql sqlalchemy graphene_sqlalchemy

# Adds support for postgres
pip install psycopg2
```

Then in the `models.py` file, make the connection string passed to the engine:

```python
engine = create_engine('postgresql+psycopg2://dbuser:dbpassword@localhost:5432/exampledb')
```
___

### Error on Schema

There is an error in the example which is caused by the class type returning the exact same name as one of the underlying types. 

Following the bug [here](https://github.com/graphql-python/graphene-sqlalchemy/issues/153) helps tremendously. 

**TL;DR**

Change this in your schema file, making sure to change all of the calls to these classes as well. 

```python
class DepartmentConnections(relay.Connection):
    ...

class EmployeeConnections(relay.Connection):
    ...
```
