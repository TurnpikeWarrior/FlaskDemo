1. create a GitHub repo  

2. conda install gunicorn  

3a. create Procfile  
`web: gunicorn app:app` 

3b. create app.py  
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

3c. create requirements.txt 
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

4. `Python test app.py` to ensure that it is working  

5. `gunicorn app:app`  

6. 