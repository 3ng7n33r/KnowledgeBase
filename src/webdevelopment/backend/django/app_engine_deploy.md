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

This will connect the project in the sdk with the db, then the cloud sql proxy file is downloaded into the project folder and given the necessary rights and then the deamon is started to make

app.yaml
main.py
settings (DB, static ...)
requirements.txt (gunicorn, psycopg2 ...)
collectstatic ...
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM2NDk2NzY4MywtMTkwNzc0MjA0NSwtND
UwMDQ2ODM2LDEzMjMxMDI3NjJdfQ==
-->