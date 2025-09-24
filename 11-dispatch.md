Dispatch is the service which dispatches the product after purchase. It is written in GoLang, So wanted to install GoLang.

Developer has chosen GoLang, Check with developer which version of GoLang is needed.

Make sure we are in root access or not to get root access
 ```
 sudo su -
 ```

Install GoLang
```
dnf install golang -y
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
curl -L -o /tmp/dispatch.zip https://roboshop-artifacts.s3.amazonaws.com/dispatch-v3.zip 
```
```
cd /app
```
To unzip
``` 
unzip /tmp/dispatch.zip
```

Lets download the dependencies & build the software.
```
cd /app 
```
```
go mod init dispatch
```
```
go get 
```
```
go build
```

We need to setup a new service in systemd so systemctl can manage this service
Setup SystemD Payment Service
we  can create file by using 
```
vim /etc/systemd/system/dispatch.service
```
```
[Unit]
Description = Dispatch Service
[Service]
User=roboshop
// highlight-start
Environment=AMQP_HOST=RABBITMQ-IP
// highlight-end
Environment=AMQP_USER=roboshop
Environment=AMQP_PASS=roboshop123
ExecStart=/app/dispatch
SyslogIdentifier=dispatch

[Install]
WantedBy=multi-user.target
```


Load the service.
```
systemctl daemon-reload
```

This above command is because we added a new service, We are telling systemd to reload so it will detect new service.

Start the service.
```
systemctl enable dispatch 
```
```
systemctl start dispatch
```