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

 - [ ] create admin:

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU3NzQxMDk1OSwxNjAwNzA5MTkzLDEyOT
kxMzUzNjNdfQ==
-->