# Django

**Start the project:**

    $ django-admin startproject mysite
   Start the app

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
eyJoaXN0b3J5IjpbLTI5ODA4ODg3NCwzOTc4MzE2ODNdfQ==
-->