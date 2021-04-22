# Deploy on Google App Engine

- Create project in GC
- Enable API: 
-- Cloud Logging API 
-- Compute Engine API 
-- Cloud SQL Admin API 
- create sql instance in GC project

Setup gcloud SDK and init project
Setup Cloud SQL Auth proxy:

    $ gcloud sql instances describe [YOUR_INSTANCE_NAME]
    $ wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O cloud_sql_proxy
    $ chmod +x cloud_sql_proxy
    $ ./cloud_sql_proxy -instances"[YOUR_INSTANCE_CONNECTION_NAME]"=tcp:3306

This will connect the project in the sdk with the db, then the cloud sql proxy file is downloaded into the project folder and given the necessary rights and then the deamon is started to make the local version connect with the upstream db

Modify settings.py so it automatically detects if it is accessed online or locally:
```py
if os.getenv('GAE_APPLICATION', None):
    # Running on production App Engine, so connect to Google Cloud SQL using
    # the unix socket at /cloudsql/<your-cloudsql-connection string>
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql', #or postgresql, sqlite3, oracle
            'HOST': '/cloudsql/[YOUR-CONNECTION-NAME]',
            'USER': '[YOUR-USERNAME]',
            'PASSWORD': '[YOUR-PASSWORD]',
            'NAME': '[YOUR-DATABASE]',
        }
    }else:
    # Running locally so connect to either a local MySQL instance or connect 
    # to Cloud SQL via the proxy.  To start the proxy via command line: 
    #    $ cloud_sql_proxy -instances=[INSTANCE_CONNECTION_NAME]=tcp:3306 
    # See https://cloud.google.com/sql/docs/mysql-connect-proxy
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql', #or postgresql, sqlite3, oracle
            'HOST': '127.0.0.1',
            'PORT': '3306',
            'NAME': '[YOUR-DATABASE]',
            'USER': '[YOUR-USERNAME]',
            'PASSWORD': '[YOUR-PASSWORD]',
        }
    }
```
You have now setup a new database connection for Django so don't forget to migrate and createsuperuser

app.yaml
```yaml
runtime: python38

handlers:
# This configures Google App Engine to serve the files in the app's
# static directory.
- url: /static
  static_dir: static/
# This handler routes all requests not caught above to the main app. 
# It is required when static routes are defined, but can be omitted 
# (along with the entire handlers section) when there are no static 
# files defined.
- url: /.*
  script: auto

entrypoint: gunicorn -b :$PORT djackets_django.wsgi
```
main.py
```py
from djackets_django.wsgi import application
# App Engine by default looks for a main.py file at the root of the app
# directory with a WSGI-compatible object called app.
# This file imports the WSGI-compatible object of the Django app,
# application from mysite/wsgi.py and renames it app so it is
# discoverable by App Engine without additional configuration.
# Alternatively, you can add a custom entrypoint field in your app.yaml:
# entrypoint: gunicorn -b :$PORT mysite.wsgi
app = application
```
settings (DB, static ...)
```py
ALLOWED_HOSTS = ['django-test-311217.ew.r.appspot.com']
...
STATIC_URL = '/static/'
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
STATIC_ROOT = 'static'
```
requirements.txt (gunicorn, psycopg2 ...)
```

```
collectstatic ...
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNzQ2ODMyNDAsLTEwNDYyMzY0NjUsLT
E5MDc3NDIwNDUsLTQ1MDA0NjgzNiwxMzIzMTAyNzYyXX0=
-->