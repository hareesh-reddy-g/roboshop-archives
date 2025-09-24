
Firstly we have to setup the mongodb repo file

Setup the MongoDB repo file 
Make sure we are in root access or not to get root access
 ```
 sudo su -
 ```

```
vim/etc/yum.repos.d/mongo.repo
```

```
[mongodb-org-7.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/9/mongodb-org/7.0/x86_64/
enabled=1
gpgcheck=0
```

Install mongodb
```
dnf install mongodb-org -y
```
Start and Enable mongodb service
```
systemctl enable mongod
```
```
systemctl start mongod
```
```
netstat -lntp
```
For mongodb default open port is 127.0.0.1.27017 but internet notation is 0.0.0.0.27017
For update notation
```
vim/etc/mongod.conf
```
Restart the service to make the changes affected
```
systemctl restart mongod
```
To check notation was changed or not 
```
netstat -lntp
```
```
systemctl status mongod
```
```
ps -ef | grep mongo
```