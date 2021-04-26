# Testing Rest Framework
## Coverage

    pip install coverage
    coverage run --source=. manage.py test
    coverage report

## Populate objects
Try [Model bakery](https://pypi.org/project/model-bakery/) for automatic generation using the models file or [faker](https://github.com/joke2k/faker/) to generate more comprehensive data manually

## Structure
To better structure the tests, we delete the tests.py file in the app folder that has been created with the startapp command and give each app its own testing folder. Within this folder, we need an \_\_init__.py file to make python recognize it as a module. Then, it makes sense to structure tests for each module (model, views, setup ...). The set up can be inherited. So the Structure should look something like this:

    Project folder
    -  App folder
    -- tests
    --- __init__.py
    --- test_setup.py
    --- test_model.py
    ...
  
## Documentation
Good Table to get an overview for the API structure:
|Endpoint 	|HTTP Method| 	CRUD Method| 	Result
|--|--|--|--|
puppies| 	GET 	|READ 	|Get all puppies
puppies/:id 	|GET 	|READ 	|Get a single puppy
puppies |	POST 	|CREATE 	|Add a single puppy
puppies/:id 	|PUT 	|UPDATE 	|Update a single puppy
puppies/:id 	|DELETE 	|DELETE 	|Delete a single puppy

## Postgres
When checking with a Postgres DB, first make sure the local DB is running:

    $ sudo -i -u postgres // switch bash to user postgres
    $ psql // start local postgres server

To make Django connect through the iden method (no PW if the user name is correct) there can be no PW, Host or PORT option in the Django DB setup

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMxMTAyOTQ5MCwtMTEwODA0OTQ4NCwtOT
Y1OTU2NTk0LDEwMjk3NDA3ODcsMTEyMzUwMzE0MiwtMTk1Njgx
NDQ2Nyw2MTQ0NTk3MDYsLTc0ODgxNTkxXX0=
-->