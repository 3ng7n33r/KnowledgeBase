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
add to setting.py -> installed apps:

    'rest_framework',
    'rest_framework.authtoken',
    'corsheaders',
    'djoser',

configure cors (add to settings.py):

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
    npm install axios <- make API calls
    npm install bulma <- CSS framework

add FontAwesome to public/index.html:

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.2/css/all.min.css">

replace style in app.vue with:

    @import  '../node_modules/bulma';
    and replace navbar according to vid @18:30

## Create Django app

python manage.py startapp product

create models ...
### Creating thumbnails of images:
To create thumbnails of images, you need to import the Pillow library, BytesIO and the File wrapper:

    from django.core.files import File
    from io import BytesIO
    from PIL import Image

and add this function to the corresponding model:

    def make_thumbnail(self, image, size=(300, 200)):
	    img = Image.open(image)
	    img.convert('RGB')
	    img.thumbnail(size)
	    thumb_io = BytesIO()
	    img.save(thumb_io, 'JPEG', quality=85)
	    thumbnail = File(thumb_io, name=image.name)
	    return thumbnail
and add media to settings.py:

    MEDIA_URL = '/media/'
    MEDIA_ROOT = BASE_DIR / 'media'
 and to urls.py:

    from django.conf import settings
    from django.conf.urls.static import static
    ...
    
    urlpatterns = [
	...
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIyNDU1MDM2NV19
-->