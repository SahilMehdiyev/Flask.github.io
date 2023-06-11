# Flask
Python Flask Simple To Do - SQLALchemy

Define Models
Subclass db.Model to define a model class. The db object makes the names in sqlalchemy and sqlalchemy.orm available for convenience, such as db.Column. The model will generate a table name by converting the CamelCase class name to snake_case.

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String, unique=True, nullable=False)
    email = db.Column(db.String)
The table name "user" will automatically be assigned to the model’s table.

See Models and Tables for more information about defining and creating models and tables.


Create the Tables
After all models and tables are defined, call SQLAlchemy.create_all() to create the table schema in the database. This requires an application context. Since you’re not in a request at this point, create one manually.

with app.app_context():
    db.create_all()
If you define models in other modules, you must import them before calling create_all, otherwise SQLAlchemy will not know about them.

create_all does not update tables if they are already in the database. If you change a model’s columns, use a migration library like Alembic with Flask-Alembic or Flask-Migrate to generate migrations that update the database schema.

Query the Data
Within a Flask view or CLI command, you can use db.session to execute queries and modify model data.

SQLAlchemy automatically defines an __init__ method for each model that assigns any keyword arguments to corresponding database columns and other attributes.

db.session.add(obj) adds an object to the session, to be inserted. Modifying an object’s attributes updates the object. db.session.delete(obj) deletes an object. Remember to call db.session.commit() after modifying, adding, or deleting any data.

db.session.execute(db.select(...)) constructs a query to select data from the database. Building queries is the main feature of SQLAlchemy, so you’ll want to read its tutorial on select to learn all about it. You’ll usually use the Result.scalars() method to get a list of results, or the Result.scalar() method to get a single result.
