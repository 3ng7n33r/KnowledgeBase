Rest - Representational state transfer
Api - application programming interface

1. The rest framework needs a serializer. The serializer explains how to format the db data into formats like json etc. It can be as simple as: 
```python
from rest_framework import serializers
from app.models import Snippet, LANGUAGE_CHOICES, STYLE_CHOICES

  
class  SnippetSerializer(serializers.ModelSerializer):
	class  Meta:
		model = Snippet
		fields = ['id', 'title', 'code', 'linenos', 'language', 'style']
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMxNjk4NDY4OSwtMjAzNTI3MTg4OV19
-->