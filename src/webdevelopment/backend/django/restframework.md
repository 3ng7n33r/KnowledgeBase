Rest - Representational state transfer
Api - application programming interface

1. The rest framework needs a serializer. The serializer explains how to format the db data into formats like json etc. It can be as simple as: 
```python
from rest_framework import serializers
from app.models import Modelname

class  AppSerializer(serializers.ModelSerializer):
	class  Meta:
		model = App
		fields = ['id', 'title', 'other', 'important', 'fields']
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MDkzMDY3ODQsLTIwMzUyNzE4ODldfQ
==
-->