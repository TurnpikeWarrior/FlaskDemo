1. Create a GitHub repo  

2. `conda install gunicorn`  

3a. Create **Procfile**   
`web: gunicorn app:app`

3b. create **app.py**  
```
from flask import Flask, jsonify

justice_league_members = [
    {"superhero": "Aquaman", "real_name": "Arthur Curry"},
    {"superhero": "Batman", "real_name": "Bruce Wayne"},
    {"superhero": "Cyborg", "real_name": "Victor Stone"},
    {"superhero": "Flash", "real_name": "Barry Allen"},
    {"superhero": "Green Lantern", "real_name": "Hal Jordan"},
    {"superhero": "Superman", "real_name": "Clark Kent/Kal-El"},
    {"superhero": "Wonder Woman", "real_name": "Princess Diana"}
]

# Flask setup
app = Flask(__name__)

@app.route("/")
def welcome():
    """List all available api routes."""
    print("Retrieving homepage")
    return "Welcome to my home page"

@app.route("/api/justice_league")
def all_justice():
    """Return a list of all passenger names"""

    print("Retrieving justice league API")
    return jsonify(justice_league_members)


if __name__ == '__main__':
    app.run(debug=False)
```

3c. **_OPTIONAL - app2.py_** 
```
from flask import Flask, jsonify, render_template
import numpy as np
import os
import sqlalchemy
from sqlalchemy.ext.automap import automap_base
from sqlalchemy.orm import Session
from sqlalchemy import create_engine, func

# Grab connection URL from local environment variables
connection_var = os.environ.get("mysql_connection")
engine = create_engine(connection_var)

# Uses local config file
# from config import connection
# connection_string = connection["mysql_connection"]
# print(connection_string)
# engine = create_engine(connection_string)


# reflect an existing database into a new model
Base = automap_base()
# reflect the tables
Base.prepare(engine, reflect=True)

# Save reference to the table
Justice = Base.classes.justice_league
# Avengers = Base.classes.avengers

# Create our session (link) from Python to the DB
session = Session(engine)

# Flask setup
app = Flask(__name__)

@app.route("/")
def welcome():
    """List all available api routes."""
    print("Retrieving homepage")
    return "Welcome to my hompage"

@app.route("/api/justice_league")
def all_justice():
    """Return a list of all passenger names"""
    print("Retrieving justice league API")
    # Query all passengers
    results = session.query(Justice).all()

    # Convert list of tuples into normal list
    all_superheros = []
    for superhero in results:
        superhero_dict = {}
        superhero_dict["superhero"] = superhero.superhero
        superhero_dict["real_name"] = superhero.real_name
        all_superheros.append(superhero_dict)

    return jsonify(all_superheros)

@app.route("/api/avengers")
def all_avengers():
    """Return a list of all passenger names"""
    print("Retrieving avengers API")
    # Query all passengers
    Avengers = Base.classes.avengers
    results = session.query(Avengers).all()

    # Convert list of tuples into normal list
    all_superheros = []
    for superhero in results:
        superhero_dict = {}
        superhero_dict["superhero"] = superhero.superhero
        superhero_dict["real_name"] = superhero.real_name
        all_superheros.append(superhero_dict)

    return jsonify(all_superheros)


if __name__ == '__main__':
    app.run(debug=False)
```

3c. create **requirements.txt**
```
click==6.7
Flask==0.12.2
gunicorn==19.7.1
itsdangerous==0.24
Jinja2==2.9.6
MarkupSafe==1.0
mysqlclient==1.3.12
numpy==1.13.3
PyMySQL==0.7.11
SQLAlchemy==1.1.14
Werkzeug==0.12.2
```

4. `python test app.py` to ensure that it is working  

5. Type `gunicorn app:app` in terminal to ensure that it is working  

6a. Create Heroku app  
6b. Select GitHub in Deployment Method  
6c. App connected to GitHub  
6d. Automatic deploys  
6e. Manual deploy  
