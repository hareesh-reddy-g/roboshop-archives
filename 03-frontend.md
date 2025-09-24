The frontend is the Micro service in RoboShop to serve the web content over Nginx. This will have the webframe for the web application.

Make sure we are in root access or not to get root access
 ```
 sudo su -
 ```

To diable previous version of package
```
 dnf module disable nginx -y
 ```
 To enable current version
 ```
 systemctl enable nginx:1.24 -y
 ```

 To install nginx
 ```
 dnf install nginx
 ```
 
 To enable current nginx
```
systemctl enable nginx
```

 To start nginx
```
systemctl start nginx
```

Remove the default content that web server is serving.
```
rm -rf /usr/share/nginx/html/* 
```

To Download the frontend content
```
curl -o /tmp/frontend.zip https://roboshop-artifacts.s3.amazonaws.com/frontend-v3.zip
```

Extract the frontend content.
```
cd /usr/share/nginx/html
```
``` 
unzip /tmp/frontend.zip
```
Try to access the nginx service once more over the browser and ensure you get roboshop content.
Create Nginx Reverse proxy configuration to reach backend services
```
vim /etc/nginx/nginx.conf
```
Add the below content in vim editor
```
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log notice;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        root         /usr/share/nginx/html;

        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }

        location /images/ {
          expires 5s;
          root   /usr/share/nginx/html;
          try_files $uri /images/placeholder.jpg;
        }
        location /api/catalogue/ { proxy_pass http://localhost:8080/; }
        location /api/user/ { proxy_pass http://localhost:8080/; }
        location /api/cart/ { proxy_pass http://localhost:8080/; }
        location /api/shipping/ { proxy_pass http://localhost:8080/; }
        location /api/payment/ { proxy_pass http://localhost:8080/; }

        location /health {
          stub_status on;
          access_log off;
        }

    }
}
```
Make sure we have to remove everything in the earlier vimeditor with ``` :/%d ``` and paste the above content 

Ensure we have to  replace the localhost  with the actual or record ip address names of those component server. 
 localhost is just used to avoid the failures on the Nginx Server.


 Create Record in Route53 in AWS Account with the name catalogue with the value of catalogue private ip address and TTL is 0 

 To check it was created or not 
 ```
 telnet catalogue.hareesh.space 8080
 ```

 To restart nginx

```
systemctl restart nginx
```

Create Another Record with name nill and value with frontend public ip address and make sure TTL is 0

Go to browser and check once with ``` hareesh.space ```

To check log
```
cd /var/log/nginx/
```

```
ls -l
```

```
tail -f access.log