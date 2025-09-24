RabbitMQ is a messaging Queue which is used by some components of the applications.
Make sure we are in root access or not to get root access
 ```
 sudo su -
 ```

Setup the RabbitMQ repo file. we can create file using
```
vim /etc/yum.repos.d/rabbitmq.repo
```
```
[modern-erlang]
name=modern-erlang-el9
baseurl=https://yum1.novemberain.com/erlang/el/9/$basearch
        https://yum2.novemberain.com/erlang/el/9/$basearch
        https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-erlang/rpm/el/9/$basearch
enabled=1
gpgcheck=0

[modern-erlang-noarch]
name=modern-erlang-el9-noarch
baseurl=https://yum1.novemberain.com/erlang/el/9/noarch
        https://yum2.novemberain.com/erlang/el/9/noarch
        https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-erlang/rpm/el/9/noarch
enabled=1
gpgcheck=0

[modern-erlang-source]
name=modern-erlang-el9-source
baseurl=https://yum1.novemberain.com/erlang/el/9/SRPMS
        https://yum2.novemberain.com/erlang/el/9/SRPMS
        https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-erlang/rpm/el/9/SRPMS
enabled=1
gpgcheck=0

[rabbitmq-el9]
name=rabbitmq-el9
baseurl=https://yum2.novemberain.com/rabbitmq/el/9/$basearch
        https://yum1.novemberain.com/rabbitmq/el/9/$basearch
        https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-server/rpm/el/9/$basearch
enabled=1
gpgcheck=0

[rabbitmq-el9-noarch]
name=rabbitmq-el9-noarch
baseurl=https://yum2.novemberain.com/rabbitmq/el/9/noarch
        https://yum1.novemberain.com/rabbitmq/el/9/noarch
        https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-server/rpm/el/9/noarch
enabled=1
gpgcheck=0
```
Here no changes in it

Install RabbitMQ
```
dnf install rabbitmq-server -y
```

Start RabbitMQ Service
```
systemctl enable rabbitmq-server
```
```
systemctl start rabbitmq-server
```

To check port 
```
netstat -lntp
```

RabbitMQ comes with a default username / password as guest/guest. But this user cannot be used to connect. Hence, we need to create one user for the application.
```
rabbitmqctl add_user roboshop roboshop123
```
```
rabbitmqctl set_permissions -p / roboshop ".*" ".*" ".*"
```

Here we have to create record for rabbitmq with name rabbitmq and value is rabbitmq private ip address and TTL is 1