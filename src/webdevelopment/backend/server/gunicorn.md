# GUnicorn

## Description

## Server config
Install gunicorn and run the daemon worker with this command:

    gunicorn --daemon --workers 3 --bind unix:/home/ubuntu/<project_name>/<project_name>.sock <project_name>.wsgi

\-\-daemon

\-\- workers : see [here](https://docs.gunicorn.org/en/latest/design.html#how-many-workers) how to set the right amount of workers. The rule of thumb is (cores x 2) + 1

\-\-bind gunicorn to the unix socket configu
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzODE2OTE3MCwxODUzODg5NjA4LDczMD
k5ODExNl19
-->