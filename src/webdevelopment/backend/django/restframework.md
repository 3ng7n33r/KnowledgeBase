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

## Authorization

To manage authorizations in the api, the first step is to include the DRF urls into the projects url file. project/urls.py:
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
Here, we inherit Djangos BasePermissions class and overwrite its has_object_permission method to only grant write access to the author of a piece. First we check if the request method is safe (aka read only aka GET, OPTIONS and HEAD). If yes, permission is granted. If not, it is checked if the user is the author. We then have to import this permission class into our views.py file to enforce it on a view level:
```py
from .permissions import IsAuthorOrReadOnly # new
...

class PostDetail(generics.RetrieveUpdateDestroyAPIView):
    permission_classes = (IsAuthorOrReadOnly,)
    ...
```
These methods work well for detail pages because it modifies the object permission. If it should be done for a list or collection, the queryset has to be overridden. More on that [here](https://www.django-rest-framework.org/api-guide/filtering/#overriding-the-initial-queryset)

## Authentication
While Authorizations manage permissions, Authentication manages login, logout and user management (create delete etc.). While Django uses a session based cookie, this is not possible for a REST API as the S in Rest stands for stateless. Therefore, every request has to be fully independent. The solution is to add a unique identifier into the request. This can be a token of any kind. DRF offers: basic, session, token, and default.

 - **Basic**: The string username:password is encoded into base64 and send under "Authorization" in the header. This should only be done through a secure https connection because the credentials can easily be stolen and reused.
 - **Session**: After basic authentication, a cookie is generated *on both sides* that will further on be used to generate an ID which will be sent in the header. The database is only hit once for credentials and they are only in the first request, however managing these sessions for multiple front ends and many users is challenging and it is a stateful approach which violates the REST principle. This is therefore not advised.
 - **Token**: Upon login, a token is created and stored only on the user side. It can be stored either in localstorage or as a cookie. The current best practice is to save it as a cooke with the httponly and Secure flags. Localstorage does not automatically add the token to the header and keeping it in both is vurlnerable for XSS attacks. The token is not stored server side. Additional features like token expiration can be set. This is currently considered the best approach. To add it 'rest_framework.authentication.TokenAuthentication', has to be added to the DEFAULT_AUTHENTICATION_CLASSES in settings.py and 'rest_framework.authtoken', has to be added to the INSTALLED_APPS
 - **Default**: The default for DRF is Session and Basic. The session is used for the browsable API while basic is used for the API itself. If we would add the default class to the settings.py file to make it explicit, it would look like this: 
	 ```py
	REST_FRAMEWORK = {
	'DEFAULT_PERMISSION_CLASSES': [
	'rest_framework.permissions.IsAuthenticated',
	],
	'DEFAULT_AUTHENTICATION_CLASSES': [ 
	'rest_framework.authentication.SessionAuthentication',
	'rest_framework.authentication.BasicAuthentication'
	],
	}
	```

## User Endpoints
To have safe user endpoints to log in and out, we will use the third party packages [dj-rest-auth](https://github.com/jazzband/dj-rest-auth) and [django-allauth](https://github.com/pennersr/django-allauth).

### dj-rest-auth
pip install dj-rest-auth

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
eyJoaXN0b3J5IjpbLTQ0NDU1NzMxOCwxNjM1MjI5MTAyLDYyMT
Y5MjcxMSwtMTk0MjcyMTI5NSwtNDU0MDIzNTIyLC0xODE5NDcx
MjEzLC0xNzcyODM2NzIwLC0xMjM1NTYwNDY2LC0xNzUyOTQxNz
c4LDE2Mjk3OTYyOTAsMjY3MTg3MDk5LC0yMDIxMjUzNDc0LDE5
MDY0NTA2MDEsLTEwODgzMzY5MzIsMzgzODA3MjAzLC0xNDY5Nj
k1Njk2LDUyNDkyMTgxNCwtMjAzNTI3MTg4OV19
-->