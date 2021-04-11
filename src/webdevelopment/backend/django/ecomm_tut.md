# Ecomm tut

[Link](https://www.youtube.com/watch?v=Yg5zkd9nm6w&list=WL&index=70)
## Setup and install Django
create repo
clone repo
create virtualenv: virtualenv env
source env/bin/activate
pip install django
pip install django-rest-framework
pip install django-cors-headers
pip install djoser
pip install pillow < -- image library
pip install stripe

django-admin startproject djackets_django
add to installed apps:
'rest_framework',
'rest_framework.authtoken',
'corsheaders',
'djoser',

configure cors:
CORS_ALLOWED_ORIGINS = [
	"http://localhost:8080",
]

configure middleware:
'corsheaders.middleware.CorsMiddleware',
#has to be above commonmiddleware

configure urls:
 from django.urls import include
urlpatterns = [
...
path('api/v1/', include('djoser.urls')),
path('api/v1/', include('djoser.urls.authtoken')),

makemigrations - migrate - creatsuperuser - Done!

## Install and setup vue
install vue on the pc if necessary:
npm install -g @vue/cli

start project:
vue create djackets_vue

Install packages:
cd djackets_vue
npm install axios
npm install bulma

add FA to public/index.html:
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.2/css/all.min.css">

replace style in app.vue with:
@import  '../node_modules/bulma';
and replace navbar according to vid @18:30




<!--stackedit_data:
eyJoaXN0b3J5IjpbNDA1MDEyMzE0XX0=
-->