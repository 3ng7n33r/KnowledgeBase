 - [ ] create virtualenvironment: $ virtualenv envprojectname
 - [ ] $ conda deactivate -> source envprojectname/bin/activate
 - [ ] pip install django
 - [ ] Create project: $ django-admin startproject projectname (use . after projectname to initialise in current folder)
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
 - [ ] create boilerplate app -> urls.py
	```
	from django.urls import path
	from api import views

	urlpatterns = [
	path('/', views.index),
	]
	```
 - [ ] create boilerplate app -> views.py
	```
	from django.shortcuts import render
	from django.http import HttpResponse

	def  index(request):
		html = "<html><body>It is now %s.</body></html>" % now
		return HttpResponse(html)
	```	
 - [ ] create admin: $ python manage.py createsuperuser
 - [ ] create models, views, templates, statics, fixtures etc.
 - [ ] register models in admin.py
 - [ ] migrate with: $ python manage.py makemigrations and $ ... migrate

<!--stackedit_data:
eyJoaXN0b3J5IjpbNDU5MjI2NTYsLTIxMjQyNDY1MzEsLTE1OT
EyMDEzOTRdfQ==
-->