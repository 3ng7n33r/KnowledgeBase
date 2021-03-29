# Nginx

## Description

## Server config
```bash
cd /etc/nginx/sites-available/
sudo nano #project-name <- create nginx config file there
```
```bash
server {
    listen 80;
    server_name XXXXXXXX; #<--Server Ipv4 address
 
    location = /favicon.ico { access_log off; log_not_found off; }
 
    location /static/ {
            root /path_to_project/<project_folder_name>; #<- folder containing static folder, normally same folder with manage.py 
    }
 
    location / {
            include proxy_params;
            proxy_pass http://unix:/path_to_project/<project_name>/<project_name>.sock;
    }
}
```
To enable the above file, run the code:

    sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIzNDkxMDc0OSwzNTQyODQ4OF19
-->