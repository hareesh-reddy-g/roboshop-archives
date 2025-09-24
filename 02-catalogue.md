Install NodeJS, By default NodeJS 16 is available, We would like to enable 20 version and install list.

we can list modules by using `dnf module list nodejs
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
Configure the application.
To add Application User.
```
useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
```
To setup app directory
```
mkdir /app
```
Download the application code to created app directory. 
```
curl -o /tmp/catalogue.zip https://roboshop-artifacts.s3.amazonaws.com/catalogue-v3.zip 
```
```
cd /app
```
To unzip
```
unzip /tmp/catalogue.zip
```
Download the dependencies
```
cd /app
```
```
npm install
```
ls -l
cd node_modules/
ls -l
cd ..
clear
ls -l

Setup SystemD Catalogue Service
```
vim /etc/systemd/system/catalogue.service
```

```
[Unit]
Description = Catalogue Service

[Service]
User=roboshop
Environment=MONGO=true
// highlight-start
Environment=MONGO_URL="mongodb://<MONGODB-SERVER-IPADDRESS>:27017/catalogue"
// highlight-end
ExecStart=/bin/node /app/server.js
SyslogIdentifier=catalogue

[Install]
WantedBy=multi-user.target
```
we have to create record for mongodb
Here in above configuration we have to replace <MONGODB-SERVER-IPADDRESS> with mongodb instance private IP address
To check record was created or not 
```
nslookup mongodb.hareesh.space
```
```
telnet mongodb.hareesh.space 27017
```
^c^c^c to exit 

To load the service
```
systemctl daemon-reload
```

To start catalogue microservice
```
systemctl start catalogue
```

To enable service
```
systemctl enable catalogue
```

To check status of service 
```
systemctl status catalogue
```

To have mongo client installed we have to setup MongoDB repo and install mongodb-client. we can create file using
```
vim /etc/yum.repos.d/mongo.repo
```
```
[mongodb-org-7.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/9/mongodb-org/7.0/x86_64/
enabled=1
gpgcheck=0
```

```
mongosh --host mongodb hareesh.space </app/db/master-data.js
```

we need to update catalogue server ip address in frontend configuration. Configuration file is 
```
vim/etc/nginx/nginx.conf
```
```
mongosh --host mongodb.hareesh.space
```
```
show dbs
```
```
use catalogue
```
```
show collections
```
```
db.products.find()
```
exit


Health Check URL
```
curl http://localhost:8080/health
```

To check the status code 
```
curl -v http://localhost:8080/health
```
Make sure app is running and ready to able to connect with database