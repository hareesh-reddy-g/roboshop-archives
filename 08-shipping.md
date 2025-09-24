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

 Load the service.
```
systemctl daemon-reload
```

This above command is because we added a new service, We are telling systemd to reload so it will detect new service.

Start and enable the service.
```
systemctl enable shipping 
```

```
systemctl start shipping
```
For this application to work fully functional we need to load schema to the Database.

Schemas are usually part of application code and developer will provide them as part of development.

We need to load the schema. To load schema we need to install mysql client.

To have it installed we can use
```
dnf install mysql -y 
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

This service needs a restart because it is dependent on schema, After loading schema only it will work as expected, Hence we are restarting this service. This

```
systemctl restart shipping
```