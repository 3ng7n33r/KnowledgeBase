## Cors
Cross origin resource  sharing is a security relevant aspect when Frontend and Backend communicate with each other from different ressources (urls, ports etc.). This attack vector can be mitigated by including specific information in the http header. For DRF, the cors middleware is suggested.

    $ pip install django-cors-headers
 and add it to the settings.py:
```py
 INSTALLED_APPS = [
	...
	'rest_framework',
	'corsheaders', # new
	...
]
MIDDLEWARE = [
...
'corsheaders.middleware.CorsMiddleware', # be sure to set it above Common
'django.middleware.common.CommonMiddleware',
...
]

CORS_ORIGIN_WHITELIST = (
'http://localhost:3000', # Frontend port, React in this case
'http://localhost:8000', # Django port
)
```

## Authentication

To add authentication to the api, the first step is to include the DRF urls into the projects url file. project/urls.py:
```py
urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('posts.urls')), # versioned API endpoint
    path('api-auth/', include('rest_framework.urls')), # adds authentication (the actual url is unimportant)
]
```
Permissions can be granted on project, view and object level. So for example to restrict permissions in a view to authenticated users this would have to be added:
```py
from rest_framework import generics, permissions

class ExampleView(generics.ListCreateAPIView):
	permission_classes = (permissions.IsAuthenticated,) # The colon at the end is important
	...
```

To change permissions on a project level, edit the projects settings.py. Options are

 - AllowAny (default)- any user, authenticated or not, has full access
 - IsAuthenticated - only authenticated, registered users have access
 - IsAdminUser - only admins/superusers have access
 - IsAuthenticatedOrReadOnly - unauthorized users can view any page,
   but only authenticated users have write, edit, or delete
   privileges:

```py
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated', # new
    ]
}
```

### Custom Permissions
It is possible to create custom permissions for a project. To do that, we create a permissions.py file in our app folder. An example for a custom permission could look like that:
```py
from rest_framework import permissions

class IsAuthorOrReadOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        # Read-only permissions are allowed for any request
        if request.method in permissions.SAFE_METHODS:
	        return True
        # Write permissions are only allowed to the author of a post
        return obj.author == request.user
```
Here, we inherit Djangos BasePermissions class and overwrite its has_object_permission method to only grant write access to the author of a piece

## Tutorial summary
Rest - Representational state transfer
Api - application programming interface

```
pip install djangorestframework
```

and add it to the Installed apps in settings.py:

    INSTALLED_APPS = [
         ....
        'rest_framework',
        ....
    ]


1. The rest framework needs a serializer. The serializer explains how to format the db data into formats like json etc. in a get request or how to format a post request for data that goes into the db. 
The normal serializer class is written the same way as the model class with special methods to handle requests (get, post etc.). Because of these similarities in structure and API duties, the class can be simplified with Modelserializers. This can be as simple as: 
	```python
	from rest_framework import serializers
	from app.models import Modelname

	class  AppSerializer(serializers.ModelSerializer):
		class  Meta:
			model = App
			fields = ['id', 'title', 'other', 'important', 'fields']
	```
	The implementation can be checked in the django shell by importing the serializer and calling *$ print(repr(Serializername()))*

2. Just like the Serializer often has similar tasks, the API IO is also normally fairly similar. Therefore, the view necessary to make the api connect with the outer world, can be simplified using generic class based views. An API that retrieves all elements of a database with a get request can be implemented like this:
	```python
	from App.models import Modelname
	from app.serializers import AppSerializer
	from rest_framework import generics
	
	class SnippetList(generics.ListCreateAPIView):
	   queryset = App.objects.all()
	   serializer_class = AppSerializer
	```
	and an API that retrieves, stores or deletes a single db entry:
	```python
	class AppDetail(generics.RetrieveUpdateDestroyAPIView):
	    queryset = App.objects.all()
	    serializer_class = AppSerializer
	```
	More about generic views can be found [here](https://www.django-rest-framework.org/api-guide/generic-views/).
	Note: in the urls file, class based views need the .asview method attached. To be able to use api suffixes (.json, .api) remember to modify the urlpatterns with
	```python 
	urlpatterns = format_suffix_patterns(urlpatterns)
	```
	3. Permissions can be handled with the [permission classes](https://www.django-rest-framework.org/api-guide/permissions/#api-reference) in the generic views (rest_framework permissions need to be imported):
	```python
	class  SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
		permission_classes = [permissions.IsAuthenticatedOrReadOnly,
		IsOwnerOrReadOnly]
	```
	In this example, "IsownerOrReadOnly" is a custom method defined in a permissions.py file. Documentation can be found [here](https://www.django-rest-framework.org/tutorial/4-authentication-and-permissions/#object-level-permissions)
	Authentication is built into the Rest framework and just needs to be included into the project level urls.py file:
	```python
	urlpatterns += [
	    path('anypath/', include('rest_framework.urls')),
	]
	```
	3. [Pagination and Hyperlinking](https://www.django-rest-framework.org/tutorial/5-relationships-and-hyperlinked-apis/) in a nutshell: Let the serializer do the work. Make sure URL names fit and let the serialiser class inherit the HyperlinkedModelSerializer.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgwMzg5MDc3NywtMTc3MjgzNjcyMCwtMT
IzNTU2MDQ2NiwtMTc1Mjk0MTc3OCwxNjI5Nzk2MjkwLDI2NzE4
NzA5OSwtMjAyMTI1MzQ3NCwxOTA2NDUwNjAxLC0xMDg4MzM2OT
MyLDM4MzgwNzIwMywtMTQ2OTY5NTY5Niw1MjQ5MjE4MTQsLTIw
MzUyNzE4ODldfQ==
-->