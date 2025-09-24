Redis is an open source ,in memory data store ,serves various purposes in modern  development due to its speed and verstality.
Cache in memory database

Make sure we are in root access or not to get root access
 ```
 sudo su -
 ```

 Disable current module 
```
dnf module disable redis -y
```

Enable required module
```
dnf module enable redis:7 -y
```

To install redis 
```
dnf install redis -y
```

To start redis
```
systemctl start redis
```

To check port
```
netstat -lntp
```

Usually Redis opens the port only to localhost(127.0.0.1) ,we need to access this service to be accessed by another server, So we need to change the config accordingly.
Update listen address from 127.0.0.1 to 0.0.0.0 in vim/etc/redis/redis.conf
```
vim/etc/redis/redis.conf
```
we have to replace with127.0.0.1 with 0.0.0.0 and also 
Update protected mode from yes to no make it like 
Protected-mode no

To restart redis
```
systemctl restart redis
```

To check port 
```
netstat -lntp
```

To enable redis
```
systemctl enable redis
```