 - [ ] create virtualenvironment: $ virtualenv envprojectname
 - [ ] $ conda deactivate -> source envprojectname/bin/activate
 - [ ] pip install django
 - [ ] Create project: $ django-admin startproject projectname (use . after projectname to initialise in current folder)
 - [ ] Create app: $ python manage.py startapp appname
 - [ ] Register app in settings.py - > INSTALLED_APPS = ['appname.apps.AppnameConfig', ]
 - [ ] add app to urls.py 
	```
	from django.contrib import admin
	from django.urls import path, include

	urlpatterns = [
		path('admin/', admin.site.urls),
		path('', include('appname.urls')),
	]
	```
 - [ ] create boilerplate app -> urls.py
	```
	from django.urls import path

	from . import views

	urlpatterns = [
	    path('', views.index),
	]
	```
 - [ ] create boilerplate app -> views.py
	```
	from django.http import HttpResponse


	def index(request):
	    return HttpResponse("Hello, world")
	```	
 - [ ] migrate	
 - [ ] create admin: $ python manage.py createsuperuser
 - [ ] create models, views, templates, statics, fixtures etc.
 - [ ] register models in admin.py
 - [ ] set Time_Zone in settings.py
 - [ ] migrate with: $ python manage.py makemigrations and $ ... migrate

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTcxNzk0NTkxLC0xNjg3OTczNjIzLC0xMD
MwNDg3NTMxLC0yMTI0MjQ2NTMxLC0xNTkxMjAxMzk0XX0=
-->