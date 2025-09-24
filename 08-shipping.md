Shipping service is responsible for finding the distance of the package to be shipped and calculate the price based on that.

Shipping service is written in Java, Hence we need to install Java.

Maven is a Java Packaging software, Hence we are going to install maven, This indeed takes care of java installation.

Make sure we are in root access or not to get root access
 ```
 sudo su -
 ```

 To install maven
 ```
dnf install maven -y
```

Add application User
```
useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
```

User roboshop is a function / daemon user to run the application. Apart from that we dont use this user to login to server.

Also, username roboshop has been picked because it more suits to our project name.

We keep application in one standard location. This is a usual practice that runs in the organization.

Lets setup an app directory.
```
mkdir /app 
```

Download the application code to created app directory.
```
curl -L -o /tmp/shipping.zip https://roboshop-artifacts.s3.amazonaws.com/shipping-v3.zip 
```
```
cd /app
```

To unzip
```
unzip /tmp/shipping.zip
```
```
ls -l
```


cd src/
ls -l
ls
cd java/
ls
cdcd main/
 instana/
ls
cd robotshop/
ls
cd shipping/
ls 
cd
cd /app
ls -l
vim pom.xml
 select & copy entirething 
cat pom.xml


mvn validate
mvn compile
ls -l
cd target/
ls -l
cd classes/
ls -l
cd com/instana/robotshop/
ls 
cd shipping/
ls 
cd /app
ls -l
mvn test
mvn package
ls -l
cd target/
ls -l
cd ..
ls-l


```
mvn clean
```
ls -l
```
mvn clean package
```
ls -l
```
cd target/
```
lsw =l
ls -l


Lets download the dependencies & build the application
```
cd /app
```

```
mvn clean package
```
```
mv target/shipping-1.0.jar shipping.jar
```
ls -l

Setup SystemD Shipping Service
we can create file by using
```
vim /etc/systemd/system/shipping.service
```
```
[Unit]
Description=Shipping Service

[Service]
User=roboshop
// highlight-start
Environment=CART_ENDPOINT=<CART-SERVER-IPADDRESS>:8080
Environment=DB_HOST=<MYSQL-SERVER-IPADDRESS>
// highlight-end
ExecStart=/bin/java -jar /app/shipping.jar
SyslogIdentifier=shipping

[Install]
WantedBy=multi-user.target
```
Here it is depending on Cart server and mysql server
So replace <CART-SERVER-IPADDRESS> with cart.hareesh.space & <MYSQL-SERVER-IPADDRESS> with mysql.hareesh.space 
 so create record name with mysql and value is mysql private ip address and TTL is 1.
 Also for shipping create record name with shipping and value is shipping private ip address and TTL is 1.

 ```
 dnf install mysql -y
 ```
```
cd sc
```
```
ls 
```
```
cd db/
```
```
ls -l
```

``` 
cat app-user.sql
```
```
cat schema.sql
```

```
mysql -h mysql.hareesh.space -uroot -pRoboShop@1 < /app/db/schema.sql
```

```
mysql -h mysql.hareesh.space -uroot -pRoboShop@1 < /app/db/app-user.sql 
```

```
mysql -h mysql.hareesh.space -uroot -pRoboShop@1 < /app/db/master-data.sql
```

Load the service.
```
systemctl daemon-reload
```

To enable and start service
```
systemctl enable shipping 
```

```
systemctl start shipping
```

To check port
```
netstat-lntp
```

curl http://localhost:8080/health