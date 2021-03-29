# Nginx

## Description

## Server config
```bash
cd /etc/nginx/sites-available/
sudo nano #project-name <- create nginx config file there
```
```
server {
    listen 80;
    server_name 0.0.0.0;
 
    location = /favicon.ico { access_log off; log_not_found off; }
 
    location /static/ {
            root /path_to_project/<project_folder_name>;
    }
 
    location / {
            include proxy_params;
            proxy_pass http://unix:/path_to_project/<project_name>/<project_name>.sock;
    }
}

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NzE1MTc5MjIsMzU0Mjg0ODhdfQ==
-->