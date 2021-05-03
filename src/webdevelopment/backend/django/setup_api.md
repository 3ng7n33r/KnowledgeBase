mkdir and git init and git set upstream (or clone repo)
virtualenv
[gitignore](https://gist.github.com/3ng7n33r/68dd8d0d45a3436423d1c417eef83190) (add virtualenv)
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
from [APPNAME].models import Post


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
# urls.py
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('[AP.urls')),
]

# appname/urls.py
from django.urls import path
from .views import UserList, UserDetail, PostList, PostDetail

urlpatterns = [
	path('users/', UserList.as_view()),
	path('users/<int:pk>/', UserDetail.as_view()),
	path('', PostList.as_view()),
	path('<int:pk>/', PostDetail.as_view()),
]

#views.py
from django.contrib.auth import get_user_model # new
from rest_framework import generics
from .models import Post
from .permissions import IsAuthorOrReadOnly
from .serializers import PostSerializer, UserSerializer

class PostList(generics.ListCreateAPIView):
	queryset = Post.objects.all()
	serializer_class = PostSerializer
	
class PostDetail(generics.RetrieveUpdateDestroyAPIView):
	permission_classes = (IsAuthorOrReadOnly,)
	queryset = Post.objects.all()
	serializer_class = PostSerializer
	
class UserList(generics.ListCreateAPIView): # new
	queryset = get_user_model().objects.all()
	serializer_class = UserSerializer
	
class UserDetail(generics.RetrieveUpdateDestroyAPIView): # new
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
eyJoaXN0b3J5IjpbLTI3Njg2MTIyNSwtOTgzMDgwNzEzLDM3OD
IwNjM2NiwxMDMxMDI3NzY0LC0xNzAzNDM3OTYsLTU0NDE1Njg1
NywtMjAwNTU2OTcxOSwtOTE0NTIyOTc0LDgzMTUyMjU0OCwtND
U2OTY0ODE1LC0xNTQ1MzAxMDEzXX0=
-->