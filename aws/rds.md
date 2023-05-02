create private db

create vpc
1. name: myvpc
2. cidr: 10.0.0.0/16

create subnets
1. vpc: myvpc
2. name: sn1
3. cidr: 10.0.1.0/24
another subnet
4. name: sn2
5. cidr: 10.0.2.0/24

create db
1. Engine: mysql
2. version: 5.7.33
3. template: free tier
4. name: mydb
5. master username: admin
6. master password: abir10101
7. vpc: myvpc
8. public access: no

generate easy-rsa key and certificate
certificates-manager
download ovpn
connect with vpn from local computer
connect to the db instance from mysql client
show grants for user 'admin';
create user 'read-only-user' identified by 'passwd'
GRANT SELECT, RELOAD, PROCESS, REFERENCES, INDEX, SHOW DATABASES, SHOW VIEW, TRIGGER ON *.* TO 'read-only-user'@'%' WITH GRANT OPTION;
