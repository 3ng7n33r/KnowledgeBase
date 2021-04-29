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

### dj-rest-auth (Login Logout, Password reset)
```py
$ pip install dj-rest-auth

#settings.py
INSTALLED_APPS = [
	...
	# 3rd party
	...
	'dj_rest_auth',
]

#urls.py
urlpatterns = [
	...
	path('api/v1/dj-rest-auth/', include('dj_rest_auth.urls')), # new
]
```
The login can then be found under http://127.0.0.1:8000/api/v1/dj-rest-auth/login/ Further there are .../logout, .../password/reset and password/reset/confirm.

### django-allauth (User Registration)
```py
$ pip install django-allauth

# settings.py
INSTALLED_APPS = [
	...
	'django.contrib.sites',
	# 3rd-party apps
	...
	'allauth',
	'allauth.account',
	'allauth.socialaccount',
	...
	'dj_rest_auth.registration',
]
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend' # this setting will send emails to the console during development
SITE_ID = 1 # allauth uses djangos sites framework (host multiple sites from one project), we only host one site but specification is needed anyway

# urls.py
urlpatterns = [
	...
	path('api/v1/dj-rest-auth/', include('dj_rest_auth.urls')),
	path('api/v1/dj-rest-auth/registration/', include('dj_rest_auth.registration.urls')),
]
```

### User API endpoint
Now that we can authenticate, we ant to add a user endpoint to our api to list all and individual users. Adding an endpoint always involves creating the serializer, creating the view, creating the url route. To add djangos usr model to the serializer, we write it like this (app/serializers.py):
```py
# app/serializers.py
from django.contrib.auth import get_user_model # new
...

class UserSerializer(serializers.ModelSerializer):
	class Meta:
		model = get_user_model()
		fields = ('id', 'username',)


# app/views.py
from django.contrib.auth import get_user_model # new
from .serializers import PostSerializer, UserSerializer # new

...
class UserList(generics.ListCreateAPIView): # new
	queryset = get_user_model().objects.all()
	serializer_class = UserSerializer
class UserDetail(generics.RetrieveUpdateDestroyAPIView): # new
	queryset = get_user_model().objects.all()
	serializer_class = UserSerializer

# posts/urls.py
...
from .views import UserList, UserDetail
urlpatterns = [
	path('users/', UserList.as_view()),
	path('users/<int:pk>/', UserDetail.as_view()),
	...
]
```

## Viewsets and Routers
At this point we can see quite a bit of repetition. The detail view and the list view look exactly identical. We can use viewsets which combine these funtionalities for the sake of a worse readability. To replace the views we have so far, we rewrite views.py like this: 
```py
# posts/views.py
from django.contrib.auth import get_user_model
from rest_framework import viewsets # new
from .models import Post
from .permissions import IsAuthorOrReadOnly
from .serializers import PostSerializer, UserSerializer

class PostViewSet(viewsets.ModelViewSet): # new
	permission_classes = (IsAuthorOrReadOnly,)
	queryset = Post.objects.all()
	serializer_class = PostSerializer
class UserViewSet(viewsets.ModelViewSet): # new
	queryset = get_user_model().objects.all()
	serializer_class = UserSerializer
```
Just like viewsets unify list and detail views, routers can simplify url routes.
```py
#before
from django.urls import path
from .views import UserList, UserDetail, PostList, PostDetail 

urlpatterns = [
    path('users/', UserList.as_view()), 
    path('users/<int:pk>/', UserDetail.as_view()), 
    path('', PostList.as_view()),
    path('<int:pk>/', PostDetail.as_view()),
# after
from django.urls import path
from rest_framework.routers import SimpleRouter
from .views import UserViewSet, PostViewSet

router = SimpleRouter()
router.register('users', UserViewSet, basename='users')
router.register('', PostViewSet, basename='posts')
urlpatterns = router.urls
```
It's not really clear if writing a little less code is worth the tradeoff in readability and customization. A good rule of thumb is to start with views and URLs. As your API grows in complexity if you find
yourself repeating the same endpoint patterns over and over again, then look to viewsets and
routers. Until then, keep things simple.

## Schemas and documentation
### Schemas
To add this, we have to install [PyYAML](https://pyyaml.org/) and [uritemplate](https://github.com/python-hyper/uritemplate)

    $ pip install pyyaml uritemplate
then create a static computer readable schema with:

    $ python manage.py generateschema > openapi-schema.yml
or we create a dynamic version that will be served to a url. For this, we have to change our projects urls.py like this:
```py
...
from rest_framework.schemas import get_schema_view # new


urlpatterns = [
	...
    path('openapi', get_schema_view( # new
        title="Blog API",
        description="A sample API for learning DRF",
        version="1.0.0"
    ), name='openapi-schema'),
]
```

### documentation

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
eyJoaXN0b3J5IjpbLTE5ODQ4MTc0NTEsLTExOTYwMjQ3MTQsLT
EwNDY4NTQ0ODEsLTEyOTA2NTgyODcsMzgxMjk1OTUyLC0xMTYy
NDI3ODI4LC0xMDUyMzU4NjE5LC03MzU5MDEwMDIsLTkzOTQzND
MwMiwxNjM1MjI5MTAyLDYyMTY5MjcxMSwtMTk0MjcyMTI5NSwt
NDU0MDIzNTIyLC0xODE5NDcxMjEzLC0xNzcyODM2NzIwLC0xMj
M1NTYwNDY2LC0xNzUyOTQxNzc4LDE2Mjk3OTYyOTAsMjY3MTg3
MDk5LC0yMDIxMjUzNDc0XX0=
-->