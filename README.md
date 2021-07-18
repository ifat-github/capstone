# Capstone Project - Casting Agency

This project is the final project of the Udacity Fullstack Nano-Degree.
The capabilities I've learned through the course are presented in this project, among them are:
Coding in Python 3, Relational Database Architecture, Modeling Data Objects with SQLAlchemy, Authentication with Auth0, Role-Based Access Control (RBAC), Testing Flask Applications, Deploying Applications and more.

The subject I chose for this project is a Casting Agency.
The Casting Agency is responsible for creating movies and managing and assigning actors to those movies. I've built a system to simplify and streamline the process of the employees in the agency.

The URL the application is hosted in is: 
https://ifats-casting-agency.herokuapp.com


### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Environment

We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organized. 
Run:
	virtualenv venv
Activate the virtual env using:
	source venv/bin/activate


#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by naviging to the `/flaskr` directory and running:

```bash
pip3 install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/) is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) and [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/) are libraries to handle the lightweight sqlite database. Since we want you to focus on auth, we handle the heavy lift for you in `./src/database/models.py`. We recommend skimming this code first so you know how to interface with the Drink model.

- [jose](https://python-jose.readthedocs.io/en/latest/) JavaScript Object Signing and Encryption for JWTs. Useful for encoding, decoding, and verifying JWTS.

## Running the server

From within the root directory first ensure you are working using your created virtual environment.

Each time you open a new terminal session, run:

```bash
export FLASK_APP=api.py;
```

To run the server, execute:

```bash
flask run --reload
```

The `--reload` flag will detect file changes and restart the server automatically.


## Endpoints

GET '/actors'
- Fetches a set of actors

GET '/movies/'
- Fetches a set of movies

DELETE '/actors/${id}'
- Deletes a specified actor using the id of the actor
- Request Arguments: id - integer
- Returns: The id of the actor deleted

POST '/actors'
- Sends a post request in order to add an actor
- Request Body: 
{
	'name': '',
	'age': '',
	'gender': ''
}
- Returns: all of the actors in the API

PATCH '/actors/${id}'
- Sends a patch request in order to edit a specific actor details
- Request Body: 
{
	'name': '',
	'age': '',
	'gender': ''
}
- Returns: all of the actors in the API

DELETE '/movies/${id}'
- Deletes a specified movie using the id of the movie
- Request Arguments: id - integer
- Returns: The id of the movie deleted


POST '/movies'
- Sends a post request in order to add a movie
- Request Body: 
{
	'title': '',
	'release_date': ''
}
- Returns: all of the movies in the API

PATCH '/movies/${id}'
- Sends a patch request in order to edit a specific movie details
- Request Body: 
{
	'title': '',
	'release_date': ''
}
- Returns: all of the movies in the API


## Permissions

Casting Assistant:
get:actors
get:movies

Casting Director:
delete:actors
get:actors
get:movies
patch:actors
patch:movies
post:actor

Executive Producer:
delete:actors
delete:movies
get:actors
get:movies
patch:actors
patch:movies
post:actors
post:movies


## Testing Instructions

To run the tests, run
```
dropdb casting_agency_test
createdb casting_agency_test
psql casting_agency_test < casting_agency_test.psql
python3 test_flaskr.py
```

### Authentication Setup

- Go to:
fsnd-ifat.eu.auth0.com/authorize?audience=casting_agency&response_type=token&client_id=2VS6WrOUWVY0lguOqpsPQKFdrkNwnPw2&redirect_uri=https://127.0.0.1:5000/login-results

- Sign into each account and make note of the JWT, with the following credentials:
	user: castingassistant@udacity.com, password: castingassistant!@#123456
	user: castingdirector@udacity.com, password: castingdirector!@#123456
	user: executiveproducer@udacity.com, password: executiveproducer!@#123456

- Update the tokens you get for each user in the setup.sh file in the base dir.
- Export in the terminal inside the virtuel enviroment, all of the tokens. (example: export EXECUTIVE_PRODUCER_TOKEN="eyJhbGci....")

## Deploy to Heroku

1. Create an account with Heroku
2. Download the Heroku CLI using: 
	brew tap heroku/brew && brew install heroku
3. Run Heroku, using:
	heroku login
4. Save your package requirements, using:
	pip freeze > requirements.txt
5. Create a bash file, using:
	touch setup.sh
and set up all of your environment variables in that file
6. Gunicorn is a pure-Python HTTP server for WSGI applications. We'll be deploying our applications using the Gunicorn webserver.
Install gunicorn using:
	pip install gunicorn
	touch Procfile

In the procfile created, write:
	web: gunicorn app:app
7. Heroku can run all your migrations to the database you have hosted on the platform. For that, we need to create a manage.py file and insert into it:
	from flask_script import Manager
	from flask_migrate import Migrate, MigrateCommand

	from app import app
	from models import db

	migrate = Migrate(app, db)
	manager = Manager(app)

	manager.add_command('db', MigrateCommand)


	if __name__ == '__main__':
	    manager.run()
8. Now we can run our local migrations using:
	python manage.py db init
	python manage.py db migrate
	python manage.py db upgrade
9. Create Heroku app run, using:
	heroku create name_of_your_app
10. Using the git url obtained from the last step, in terminal run:
	git remote add heroku heroku_git_url
11. Run this code in order to create your database and connect it to your application: 		heroku addons:create heroku-postgresql:hobby-dev --app name_of_your_application
12. Run:
	heroku config --app name_of_your_application 
in order to check your configuration variables in Heroku.
13. Run:
	git push heroku master
14. Run migrations by running: 
	heroku run python manage.py db upgrade --app name_of_your_application

Now you have a live application!
