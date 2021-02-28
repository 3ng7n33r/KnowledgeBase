 - [ ] create virtualenvironment: $ virtualenv envprojectname
 - [ ] $ conda deactivate -> source envprojectname/bin/activate
 - [ ] pip install django
 - [ ] Create project: $ django-admin startproject projectname
 - [ ] Create app: $ python manage.py startapp appname
 - [ ] Register app in settings.py - > INSTALLED_APPS = ['appname.apps.AppnameConfig', ]
 - [ ] add app to urls.py 
```
from django.urls import include

urlpatterns = [
    path('catalog/', include('catalog.urls')),
    ...
]
```

 - [ ] create admin: $ python manage.py createsuperuser
 - [ ] create models, views, templates, statics, fixtures etc.
 - [ ] register models in admin.py
 - [ ] migrate with: $ python manage.py makemigrations and $ ... migrate

<!--stackedit_data:
eyJoaXN0b3J5IjpbNzM2NjM1OTk4XX0=
-->