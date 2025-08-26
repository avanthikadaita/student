---
layout: post
toc: True
title: Data | SQLAlchemy
categories: ['CSP Big Idea 2']
description: Using Programs with Data is often focused on SQL and database actions.  This blog focuses on SQLAlchemy and an OOP programming style.
courses: {'csp': {'week': 17}}
type: ccc
---

## Database and SQLAlchemy

Using programs with data is often focused on SQL and database actions. Create, Read, Update, and Delete (CRUD) are the foundation operations.

> This blog focuses on SQLAlchemy and an Object-Oriented Programming (OOP) style. SQLAlchemy is a powerful SQL toolkit and Object-Relational Mapping (ORM) library for Python. It allows developers to interact with databases using Python objects, making database operations more intuitive and easier to manage.

![SQLAlchemy Logo](https://www.sqlalchemy.org/img/sqla_logo.png)

### How does a database meet College Board requirements?

- **Program Usage**: Applications should have an "iterative and interactive way when processing information".
- **Managing Data**: In general, "classifying data are part of the process in using programs". Often examples talk of "data files in a Table".
- **Machine Learning**: Programs show that "insight and knowledge can be obtained from ... digitally represented information".
- **Filtering Systems**: Develop "tools for finding information and recognizing patterns".
- **Application**: In a sample program, it discusses that "the preserve has two databases". Another example says "an employee wants to count the number of books".

By incorporating Flask and SQLAlchemy elements, you can ensure that your projects align with College Board requirements and demonstrate the effective use of databases in programming.

## Imports and Flask Objects

> Defines and key object creations

To get started with Flask and SQLAlchemy, we need to import the necessary modules and create key objects. 

To learn big system review the `__init__.py` file and observe these elements in the code:

1. **Flask app object**: This object is the core of your Flask application. It is responsible for handling requests and responses.
2. **SQLAlchemy db object**: This object is the interface to your database. It allows you to define models and interact with the database using Python objects.

### Code Example


```python
"""
These imports define the key modules 
"""

from flask import Flask
from flask_sqlalchemy import SQLAlchemy

"""
These object and definitions are used to setup db with Flask
"""

# Setup of key Flask object (app)
app = Flask(__name__)
# Setup SQLAlchemy object and properties for the database (db)
database = 'sqlite:///sqlite.db'  # path and filename of database
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
app.config['SQLALCHEMY_DATABASE_URI'] = database
app.config['SECRET_KEY'] = 'SECRET_KEY'
db = SQLAlchemy()


# This belongs in place where it runs once per project
db.init_app(app)

```

## Model Definition

> Define columns, initialization, and CRUD methods for the users table in `sqlite.db`.

In this section, we will define the `User` model, which represents the users table in our SQLite database. Review the following key pieces:

- **class User**: The model class representing the users table. Look at the schema.
- **db.Model inheritance**: Inherits from SQLAlchemy's base class to provide ORM capabilities.
- **__init__ method**: Initializes the model's attributes.
- **@property, @<column>.setter**: Decorators for defining getter and setter methods for model attributes.
- **create, read, update, delete methods**: Methods for performing CRUD operations on the users table.

### Code Example


```python
""" database dependencies to support sqlite examples """
import datetime
from datetime import datetime
import json

from sqlalchemy.exc import IntegrityError
from werkzeug.security import generate_password_hash, check_password_hash


''' Tutorial: https://www.sqlalchemy.org/library.html#tutorials, try to get into a Python shell and follow along '''

# Define the User class to manage actions in the 'users' table
# -- Object Relational Mapping (ORM) is the key concept of SQLAlchemy
# -- a.) db.Model is like an inner layer of the onion in ORM
# -- b.) User represents data we want to store, something that is built on db.Model
# -- c.) SQLAlchemy ORM is layer on top of SQLAlchemy Core, then SQLAlchemy engine, SQL
class User(db.Model):
    __tablename__ = 'users'  # table name is plural, class name is singular

    # Define the User schema with "vars" from object
    id = db.Column(db.Integer, primary_key=True)
    _name = db.Column(db.String(255), unique=False, nullable=False)
    _uid = db.Column(db.String(255), unique=True, nullable=False)
    _password = db.Column(db.String(255), unique=False, nullable=False)
    _dob = db.Column(db.Date)

    # constructor of a User object, initializes the instance variables within object (self)
    def __init__(self, name, uid, password="123qwerty", dob=datetime.today()):
        self._name = name    # variables with self prefix become part of the object, 
        self._uid = uid
        self.set_password(password)
        if isinstance(dob, str):  # not a date type     
            dob = date=datetime.today()
        self._dob = dob

    # a name getter method, extracts name from object
    @property
    def name(self):
        return self._name
    
    # a setter function, allows name to be updated after initial object creation
    @name.setter
    def name(self, name):
        self._name = name
    
    # a getter method, extracts uid from object
    @property
    def uid(self):
        return self._uid
    
    # a setter function, allows uid to be updated after initial object creation
    @uid.setter
    def uid(self, uid):
        self._uid = uid
        
    # check if uid parameter matches user id in object, return boolean
    def is_uid(self, uid):
        return self._uid == uid
    
    @property
    def password(self):
        return self._password[0:10] + "..." # because of security only show 1st characters

    # update password, this is conventional method used for setter
    def set_password(self, password):
        """Create a hashed password."""
        self._password = generate_password_hash(password, method='sha256')

    # check password parameter against stored/encrypted password
    def is_password(self, password):
        """Check against hashed password."""
        result = check_password_hash(self._password, password)
        return result
    
    # dob property is returned as string, a string represents date outside object
    @property
    def dob(self):
        dob_string = self._dob.strftime('%m-%d-%Y')
        return dob_string
    
    # dob setter, verifies date type before it is set or default to today
    @dob.setter
    def dob(self, dob):
        if isinstance(dob, str):  # not a date type     
            dob = date=datetime.today()
        self._dob = dob
    
    # age is calculated field, age is returned according to date of birth
    @property
    def age(self):
        today = datetime.today()
        return today.year - self._dob.year - ((today.month, today.day) < (self._dob.month, self._dob.day))
    
    # output content using str(object) is in human readable form
    # output content using json dumps, this is ready for API response
    def __str__(self):
        return json.dumps(self.read())

    # CRUD create/add a new record to the table
    # returns self or None on error
    def create(self):
        try:
            # creates a person object from User(db.Model) class, passes initializers
            db.session.add(self)  # add prepares to persist person object to Users table
            db.session.commit()  # SqlAlchemy "unit of work pattern" requires a manual commit
            return self
        except IntegrityError:
            db.session.remove()
            return None

    # CRUD read converts self to dictionary
    # returns dictionary
    def read(self):
        return {
            "id": self.id,
            "name": self.name,
            "uid": self.uid,
            "dob": self.dob,
            "age": self.age,
        }

    # CRUD update: updates user name, password, phone
    # returns self
    def update(self, name="", uid="", password=""):
        """only updates values with length"""
        if len(name) > 0:
            self.name = name
        if len(uid) > 0:
            self.uid = uid
        if len(password) > 0:
            self.set_password(password)
        db.session.add(self) # performs update when id exists
        db.session.commit()
        return self

    # CRUD delete: remove self
    # None
    def delete(self):
        db.session.delete(self)
        db.session.commit()
        return None
    
```

## Initial Data

> Uses SQLAlchemy `db.create_all()` to initialize rows into `sqlite.db`.

In this section, we will preview the code that adds initial records to the users table. This is done using a separate `db_init.py` script. Review the following key pieces:

1. **Create All Tables from db Object**: Initializes the database and creates all tables defined in the models.
2. **User Object Constructors**: Creates instances of the `User` model with initial data.
3. **Try / Except**: Handles potential errors during the database operations.

### Code Example


```python
"""Database Creation and Testing """

def initUsers():
    with app.app_context():
        """Create database and tables"""
        db.create_all()
        """Tester data for table"""
        
        u1 = User(name='Thomas Edison', uid=app.config['ADMIN_USER'], password=app.config['ADMIN_PASSWORD'], pfp='toby.png', kasm_server_needed=True, role="Admin")
        u2 = User(name='Grace Hopper', uid=app.config['DEFAULT_USER'], password=app.config['DEFAULT_PASSWORD'], pfp='hop.png', kasm_server_needed=False)
        u3 = User(name='Nicholas Tesla', uid='niko', password='123niko', pfp='niko.png', kasm_server_needed=False)
        users = [u1, u2, u3]
        
        for user in users:
            try:
                user.create()
            except IntegrityError:
                '''fails with bad or duplicate data'''
                db.session.remove()
                print(f"Records exist, duplicate email, or error: {user.uid}")

        s1 = Section(name='Computer Science A', abbreviation='CSA')
        s2 = Section(name='Computer Science Principles', abbreviation='CSP')
        s3 = Section(name='Engineering Robotics', abbreviation='Robotics')
        s4 = Section(name='Computer Science and Software Engineering', abbreviation='CSSE')
        sections = [s1, s2, s3, s4]
        
        for section in sections:
            try:
                section.create()    
            except IntegrityError:
                '''fails with bad or duplicate data'''
                db.session.remove()
                print(f"Records exist, duplicate email, or error: {section.name}")
            
        u1.add_section(s1)
        u1.add_section(s2)
        u2.add_section(s2)
        u2.add_section(s3)
        u3.add_section(s4)
```

## Check for Given Credentials in Users Table in `sqlite.db`

> Use of ORM Query object and custom methods to identify user credentials by `uid` and `password`.

In this code, review how to query the users table to check for given credentials. This demonstrates the capabilities of SQLAlchemy's ORM for querying the database. Review the following key pieces:

1. **User.query.filter_by**: Filters the query results based on specified criteria.
2. **user.password**: Accesses the password attribute of the user object to verify credentials.

### Code Example


```python
# SQLAlchemy extracts single user from database matching User ID
def find_by_uid(uid):
    with app.app_context():
        user = User.query.filter_by(_uid=uid).first()
    return user # returns user object

# Check credentials by finding user and verify password
def check_credentials(uid, password):
    # query email and return user record
    user = find_by_uid(uid)
    if user == None:
        return False
    if (user.is_password(password)):
        return True
    return False
        
#check_credentials("indi", "123qwerty")
```

## Create a New User in Table in `sqlite.db`

> Uses SQLAlchemy and custom `user.create()` method to add a row.

In this section, review how to create a new user in the users table. This demonstrates the capabilities of SQLAlchemy's ORM for adding records to the database. Review the following key pieces:

1. **user.find_by_uid() and try/except**: Checks if a user with the given UID already exists and handles potential errors.
2. **user = User(...)**: Creates a new instance of the `User` model with the provided data.
3. **user.dob and try/except**: Sets the date of birth attribute and handles potential errors.
4. **user.create() and try/except**: Adds the new user to the database and handles potential errors.

### Code Example


```python
# Inputs, Try/Except, and SQLAlchemy work together to build a valid database object
def create():
    # optimize user time to see if uid exists
    uid = input("Enter your user id:")
    user = find_by_uid(uid)
    try:
        print("Found\n", user.read())
        return
    except:
        pass # keep going
    
    # request value that ensure creating valid object
    name = input("Enter your name:")
    password = input("Enter your password")
    
    # Initialize User object before date
    user = User(name=name, 
                uid=uid, 
                password=password
                )
    
    # create user.dob, fail with today as dob
    dob = input("Enter your date of birth 'YYYY-MM-DD'")
    try:
        user.dob = datetime.strptime(dob, '%Y-%m-%d').date()
    except ValueError:
        user.dob = datetime.today()
        print(f"Invalid date {dob} require YYYY-mm-dd, date defaulted to {user.dob}")
           
    # write object to database
    with app.app_context():
        try:
            object = user.create()
            print("Created\n", object.read())
        except:  # error raised if object not created
            print("Unknown error uid {uid}")
        
create()
```

## Reading Users Table in `sqlite.db`

> Uses SQLAlchemy `query.all` method to read data.

In this section, review how to read data from the users table. This demonstrates the capabilities of SQLAlchemy's ORM for querying the database. Review the following key pieces:

1. **User.query.all**: Retrieves all records from the users table.
2. **json_ready assignment, google List Comprehension**: Converts the query results into a JSON-ready format using list comprehension.

### Code Example


```python

# SQLAlchemy extracts all users from database, turns each user into JSON
def read():
    with app.app_context():
        table = User.query.all()
    json_ready = [user.read() for user in table] # "List Comprehensions", for each user add user.read() to list
    return json_ready

read()
```

## Conclusion

In this blog, we have explored the basics of using SQLAlchemy with Flask to interact with a SQLite database. We covered:

- Setting up Flask and SQLAlchemy
- Defining models and creating tables
- Adding initial data to the database
- Querying the database to check for credentials
- Creating new records in the database
- Reading data from the database

### Hacks

- **Review CRUD Operations through Scripts**: Analyze the provided scripts to understand their functionality. You will need to use db_init.py, others if you wish to preserve data between updates.
    - Look at `db_backup.py`
    - Look at `db_init.py`
    - Look at `db_restore.py`

- **Build your own Table**: This is the small step to transition from using static data structures to dynamic database tables.  First you will need a table.
    - Using model/user.py as an example, build your own table.  
        - Try to build table with same columns / keys as you used on your RESTful API with static data.
        - Be sure to initialize some data as shown in mode/user.py
        - Observe results by opening the db file
