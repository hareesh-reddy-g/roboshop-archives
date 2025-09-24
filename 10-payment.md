This service is responsible for payments in RoboShop e-commerce app. This service is written on Python 3.x, So need it to run this app.
Make sure we are in root access or not to get root access
 ```
 sudo su -
 ```
 
Install Python 3
```
dnf install python3 gcc python3-devel -y
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
curl -L -o /tmp/payment.zip https://roboshop-artifacts.s3.amazonaws.com/payment-v3.zip 
```
```
cd /app 
```
To unzip
```
unzip /tmp/payment.zip
```

```
cat requirements.txt
```

Lets download the dependencies.
```
cd /app 
```
```
pip3 install -r requirements.txt
```

We need to setup a new service in systemd so systemctl can manage this service
Setup SystemD Payment Service
we can create file by using 
```
vim /etc/systemd/system/payment.service
```
```
[Unit]
Description=Payment Service

[Service]
User=root
WorkingDirectory=/app
// highlight-start
Environment=CART_HOST=<CART-SERVER-IPADDRESS>
Environment=CART_PORT=8080
Environment=USER_HOST=<USER-SERVER-IPADDRESS>
Environment=USER_PORT=8080
Environment=AMQP_HOST=<RABBITMQ-SERVER-IPADDRESS>
// highlight-end
Environment=AMQP_USER=roboshop
Environment=AMQP_PASS=roboshop123

ExecStart=/usr/local/bin/uwsgi --ini payment.ini
ExecStop=/bin/kill -9 $MAINPID
SyslogIdentifier=payment

[Install]
WantedBy=multi-user.target
```
Here payment is depending on cart and user and rabbitmq.
So replace <CART-SERVER-IPADDRESS> with cart.hareesh.space
           <USER-SERVER-IPADDRESS> with user.hareesh.space
           <RABBITMQ-SERVER-IPADDRESS> with rabbitmq.hareesh.space

Load the service.
```
systemctl daemon-reload
```

This above command is because we added a new service, We are telling systemd to reload so it will detect new service.

Start the service.
```
systemctl enable payment 
```
```
systemctl start payment
```

To check port 
```
netstat -lntp
```
```
curl http://localhost:8080/health