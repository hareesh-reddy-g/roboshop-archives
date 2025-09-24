User is a microservice that is responsible for User Logins and Registrations Service in RobotShop e-commerce portal.
Install NodeJS, By default NodeJS 16 is available, We would like to enable 20 version and install list.

Make sure we are in root access or not to get root access
 ```
 sudo su -
 ```

 Disable current module 
```
dnf module disable nodejs -y
```

Enable required module
```
dnf module enable nodejs:20 -y
```

To install nodejs 
```
dnf install nodejs -y
```

 Add application User . Configure the application. 
 ```
 useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
 ```

  setup an app directory
  ```
  mkdir /app
  ```

  Download the application code to created app directory.
  ```curl -L -o /tmp/user.zip https://roboshop-artifacts.s3.amazonaws.com/user-v3.zip 
```
```
cd /app
```

``` 
unzip /tmp/user.zip
```

Lets download the dependencies.
```
cd /app
```
```
npm install
```

we can create file by using vim /etc/systemd/system/user.service
Setup SystemD User Service
```
[Unit]
Description = User Service
[Service]
User=roboshop
Environment=MONGO=true
// highlight-start
Environment=REDIS_URL='redis://<REDIS-IP-ADDRESS>:6379'
Environment=MONGO_URL="mongodb://<MONGODB-SERVER-IP-ADDRESS>:27017/users"
// highlight-end
ExecStart=/bin/node /app/server.js
SyslogIdentifier=user

[Install]
WantedBy=multi-user.target
```
user component is depending on Redis and mongodb
Here in vim editor have to give redis ip address name and mongodb server ip address name.
So we have to create record for redis with name redis and valuse is redis private ip address and TTL is 1 .
So replace <REDIS-IP-ADDRESS> with redis.hareesh.space & <MONGODB-SERVER-IP-ADDRESS> with mongodb.hareesh.space 

Load the service.
```
systemctl daemon-reload
```

Enable the service
```
systemctl enable user
```

start the service
``` 
systemctl start user
```

To check the status
```
Systemctl status user
```

To check port 
```
netstat -lntp
```

```
telnet redis.hareesh.space 6379
```
```
telnet mongodb.hareesh.spacen 27017
```

 To quit from telnet
``` ctrl +] ```and then quit

```
curl -v http://localhost:8080/health
```