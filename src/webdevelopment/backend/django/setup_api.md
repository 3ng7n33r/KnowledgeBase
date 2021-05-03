mkdir
virtualenv
pip install django
django-admin startproject config .
python manage.py startapp appname

add app to installed apps: 'appname.apps.AppnameConfig',

migrate

create model
makemigrations
migrate
add to admin admin.site.register(Appname)
createsuperuser
create dummy db entries
delete test.py
create __init__.py in appname/tests
create first assert test for db in test_models.py
python manage.py test

pip install django-rest-framework
add rest_framework to settings.py
add path('api/v1/', include('appname.urls')), into config/urls.py
add:
```py
# appname/urls.py
from django.urls import path
from rest_framework.routers import SimpleRouter
from .views import UserViewSet, PostViewSet

router = SimpleRouter()
router.register('users', UserViewSet, basename='users')
router.register('', PostViewSet, basename='posts')
urlpatterns = router.urls

#views.py
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

#serializers.py
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDEzNDIxNTQ2LC0xNTQ1MzAxMDEzXX0=
-->