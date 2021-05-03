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
create __init__.py in appname/tests
create first assert test for db in test_models.py
python manage.py test

pip install django-rest-framework
add rest_framework to settings.py
add path('api/v1/', include('posts.urls')), into config/urls.py



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMzY5NzA2NjMsLTE1NDUzMDEwMTNdfQ
==
-->