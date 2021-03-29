# Server

 - Run local testing server with Python ([Link](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/set_up_a_local_testing_server))

## apply updates

 1. Pull and overwrite local changes
```bash
git add .
git stash
git pull
```
2. compile static files
```bash
python manage.py collectstatic
```
3. reload nginx and gunicorn
```bash
sudo systemctl reload nginx
kill -HUP 944 #<---944 = pid of Gunicorn main process/
ps -aux | grep gunicorn #<--- to find pid
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMzMTY4OTQ3MywxOTU2MjM4NzU3LDU1NT
c4MTUzN119
-->