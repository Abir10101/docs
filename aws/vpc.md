Setup a secure vpc network for application and db deployment

create vpc
1. vpc name myvpc
2. vpc ip range 10.0.0.0/16

create subnets
1. choose vpc in which subnet will create myvpc
2. cidr range within the range of vpc ips 10.0.1.0/24
3. two subnets ip range must not overlap with each other 10.0.2.0/24

create internet gateway
1. gateway name myigw
2. atach myigw with vpc myvpc

create route table
1. route table name pubrt
2. add routes any ip address( 0.0.0.0/0 ) request will forward to internet gateway myigw
3. add another route table prirt. subnet association prisn.

spin ec2 machine. machine in prisn cannot connect thorugh ssh. machine in pubsn can connect trough ssh

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
