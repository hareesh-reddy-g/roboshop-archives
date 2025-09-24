Install MySQL Server
Make sure we are in root access or not to get root access
 ```
 sudo su -
 ```

 To install mysql
 ```
dnf install mysql-server -y
```

To enable mysql
```
systemctl enable mysqld
```

To start mysql
```
systemctl start mysqld
```

To check port
```
netstat -lntp
```

```
ls -l /etc/my.conf
```

```
vim /etc/my.conf
```
By default it will take no need to change anything

```
cd /etc/my.cnf.d/
```
```
ls -l
```
```
vim mysql -server.cnf
```

```
vim client.cnf
```
Here also no change

```
cd
```

```
mysql_secure_installation --set-root-pass RoboShop@1
```
```
mysql
```
```
show databases;
```

exit 
