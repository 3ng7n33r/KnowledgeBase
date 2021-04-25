# Testing Rest Framework

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
eyJoaXN0b3J5IjpbLTE5NTY4MTQ0NjcsNjE0NDU5NzA2LC03ND
g4MTU5MV19
-->