### Cors
Cross origin resource  sharing is a security relevant aspect when Frontend and Backend communicate with each other from different ressources (urls, ports etc.). This attack vector can be mitigated by including specific information in the http header. For DRF, the cors middleware is suggested.

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
eyJoaXN0b3J5IjpbMzcwODMzNzEwLC0yMDIxMjUzNDc0LDE5MD
Y0NTA2MDEsLTEwODgzMzY5MzIsMzgzODA3MjAzLC0xNDY5Njk1
Njk2LDUyNDkyMTgxNCwtMjAzNTI3MTg4OV19
-->