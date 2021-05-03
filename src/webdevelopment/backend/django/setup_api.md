mkdir
virtualenv
gitignore (add virtualenv)
pip install django
django-admin startproject config .


(from app_engine_deploy)
Create GCP project
add PostgreSQL server 
setup Cloud SQL Proxy
migrate

python manage.py startapp appname
add app to installed apps: 'appname.apps.AppnameConfig',

create model
```py
from django.db import models
from django.contrib.auth.models import User


class Post(models.Model):
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    title = models.CharField(max_length=50)
    body = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    def __str__(self):
        return self.title
```
makemigrations
migrate
add to app/admin.py: 

    from .models import Modelname
    admin.site.register(Modelname)

createsuperuser
create dummy db entries
delete test.py
create __init__.py in appname/test
create first assert test for db in test_models.py
```py
from django.test import TestCase
from django.contrib.auth.models import User
from posts.models import Post


class BlogTests(TestCase):

    @classmethod
    def setUpTestData(cls):
        # Create a user
        testuser1 = User.objects.create_user(
        username='testuser1', password='abc123')
        testuser1.save()
        # Create a blog post
        test_post = Post.objects.create(
        author=testuser1, title='Blog title', body='Body content...')
        test_post.save()


    def test_blog_content(self):
        post = Post.objects.get(id=1)
        author = f'{post.author}'
        title = f'{post.title}'
        body = f'{post.body}'

        self.assertEqual(author, 'testuser1')
        self.assertEqual(title, 'Blog title')
        self.assertEqual(body, 'Body content...')
```
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
from rest_framework import serializers
from django.contrib.auth import get_user_model
from .models import Post


class PostSerializer(serializers.ModelSerializer):
    class Meta:
        fields = ('id', 'author', 'title', 'body', 'created_at',)
        model = Post


class UserSerializer(serializers.ModelSerializer):
	class Meta:
		model = get_user_model()
		fields = ('id', 'username',)
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MDM0Mzc5NiwtNTQ0MTU2ODU3LC0yMD
A1NTY5NzE5LC05MTQ1MjI5NzQsODMxNTIyNTQ4LC00NTY5NjQ4
MTUsLTE1NDUzMDEwMTNdfQ==
-->