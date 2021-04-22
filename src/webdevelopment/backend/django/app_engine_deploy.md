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



app.yaml
main.py
settings (DB, static ...)
requirements.txt (gunicorn, psycopg2 ...)
collectstatic ...
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NTY5NDg3NDYsLTE5MDc3NDIwNDUsLT
Q1MDA0NjgzNiwxMzIzMTAyNzYyXX0=
-->