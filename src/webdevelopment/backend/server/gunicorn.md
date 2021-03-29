# GUnicorn

## Description

## Server config
Install gunicorn and run the daemon worker with this command:

    gunicorn --daemon --workers 3 --bind unix:/home/ubuntu/<project_name>/<project_name>.sock <project_name>.wsgi

The command has to be executed in the parent directory of the Django project (the directory that contains manage.py)

\-\-daemon: Leaves the process running in the background. Should be substituted by Supervisor?

\-\- workers : see [here](https://docs.gunicorn.org/en/latest/design.html#how-many-workers) how to set the right amount of workers. The rule of thumb is (cores x 2) + 1

\-\-bind gunicorn to the unix socket set in the nginx configuration


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg0NDA4MzM0MywxNzk0OTA5ODQ5LDE4NT
M4ODk2MDgsNzMwOTk4MTE2XX0=
-->