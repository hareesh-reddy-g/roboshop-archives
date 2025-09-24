Cart is a microservice that is responsible for Cart Service in RobotShop e-commerce portal.
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

Add application User
```
useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
```

Lets setup an app directory.
```
mkdir /app
```

Download the application code to created app directory.
```
curl -L -o /tmp/cart.zip https://roboshop-artifacts.s3.amazonaws.com/cart-v3.zip
```
```
cd /app 
```
To unzip
```
unzip /tmp/cart.zip
```

Lets download the dependencies.
```
cd /app
``` 
```
npm install
```

We need to setup a new service in systemd so systemctl can manage this service.
Setup SystemD Cart Service.
we can create file by using 
```
vim /etc/systemd/system/cart.service
```
```
[Unit]
Description = Cart Service
[Service]
User=roboshop
// highlight-start
Environment=REDIS_HOST=<REDIS-SERVER-IP>
Environment=CATALOGUE_HOST=<CATALOGUE-SERVER-IP>
Environment=CATALOGUE_PORT=8080
// highlight-end
ExecStart=/bin/node /app/server.js
SyslogIdentifier=cart

[Install]
WantedBy=multi-user.target
```
Here the cart is depending on redis and catalogue.So we have to create record for redis with name redis and value is redis private ip address and TTL is 1 .
So replace <REDIS-SERVER-IP with redis.hareesh.space & <CATALOGUE-SERVER-IP>with mongodb.hareesh.space 

Load the service.
```
systemctl daemon-reload
```

This above command is because we added a new service, We are telling systemd to reload so it will detect new service.

To enable & Start the service.
```
systemctl enable cart 
```
```
systemctl start cart
```

Health check is 
```
curl http://localhost:8080/health
```

We have to create record for this cart with name cart and value with cart private ip address and TTL is 1