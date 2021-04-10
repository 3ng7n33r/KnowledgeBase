# Django

**Start the project:**
Prerequisite:
sudo apt install python3-django


    $ django-admin startproject mysite 
    #add a . after the project name to use current directory
   Start the app
   Prerequisite:

       $ virtualenv env
       $ source env/bin/activate
       $ pip install django

    $ python manage.py startapp myapp
Make migrations (even if you're not using a database, you need to populate Django's sqlite file)

    $ python manage.py migrate

add app and template directory to settings.py: 

> INSTALLED_APPS = [
> 'myapp.apps.MyappConfig',
...
> ]
> TEMPLATES = [{
>     'BACKEND': 'django.template.backends.django.DjangoTemplates',
>     'DIRS': [BASE_DIR / 'templates'],
>     ...
>     },]

**Create an admin page**

    $ python manage.py createsuperuser

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NzA4NDQzMzgsMzk3ODMxNjgzXX0=
-->