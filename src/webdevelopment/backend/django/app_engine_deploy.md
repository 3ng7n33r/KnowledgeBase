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


app.yaml
main.py
settings (DB, static ...)
requirements.txt (gunicorn, psycopg2 ...)
collectstatic ...
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MDc3NDIwNDUsLTQ1MDA0NjgzNiwxMz
IzMTAyNzYyXX0=
-->