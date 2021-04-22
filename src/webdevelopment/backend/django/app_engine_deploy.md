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
main.py
settings (DB, static ...)
requirements.txt (gunicorn, psycopg2 ...)
collectstatic ...
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNDYyMzY0NjUsLTE5MDc3NDIwNDUsLT
Q1MDA0NjgzNiwxMzIzMTAyNzYyXX0=
-->