Rest - Representational state transfer
Api - application programming interface

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
	

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYzMjUyODYwMCwtMjAzNTI3MTg4OV19
-->