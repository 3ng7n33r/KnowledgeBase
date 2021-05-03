mkdir
virtualenv
pip install django
django-admin startproject config .
python manage.py startapp appname

add app to installed apps: 'appname.apps.AppnameConfig',

migrate

create model
makemigrations
migrate
add to admin 

pip install django-rest-framework

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc1NDAzMzUxOF19
-->