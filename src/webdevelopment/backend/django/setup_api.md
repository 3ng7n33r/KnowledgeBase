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
add to admin admin.site.register(Appname)
createsuperuser
create dummy db entries
delete test.py
create __init__.py in 

pip install django-rest-framework

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgxMjg0NTU2Nl19
-->