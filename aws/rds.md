# Setup a secure database like vawsum

### Step 1: Create a vpc
- name: `myvpc`
- cidr: `10.0.0.0/16`

### Step 2: Create public and private subnets
- vpc_id: `myvpc`
- name: `pubsn`
- cidr: `10.0.1.0/24`  
<ins> add another sn </ins>
- name: `prisn`
- cidr: `10.0.2.0/24`

Note: two subnets ip range must not overlap with each other.

### Step 3: Create rds
- engine: `mysql`
- version: `5.7.33`
- template: `free tier`
- name: `mydb`
- master username: `admin`
- master password: `abir10101`
- vpc: `myvpc`
- subnet: `sn2`
- public access: `no`

### Step 4: Generate easy-rsa key and certificate
- follow: [Amazon docs](https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/mutual.html)

### Step 5: Import certificates in aws
- certificate manager &#8594; import certificate

<ins> import server crt </ins>
- certificate body: `cp easy-rsa/pki/issued/server.crt`
- certificate private key: `cp easy-rsa/pki/private/server.key`
- certificate chain: `cp easy-rsa/pki/ca.crt`

<ins> import client crt </ins>
- certificate body: `cp easy-rsa/pki/issued/server.crt`
- certificate private key: `cp easy-rsa/pki/private/server.key`
- certificate chain: `cp easy-rsa/pki/ca.crt`

Note: two certificate names will be server and client

### Step 5: Create cpn endpoint
- vpc &#8594; client VPN endpoint
- Client IPv4 CIDR: `10.0.0.0/22`
- Server certificate ARN: `server crt`
- Server certificate ARN: `true`
- Client certificate ARN: `client crt`
- associations tab
- vpc: `myvpc`
- subnet: `sn2`
- download vpn file (.ovpn)

### Step 6: Connect with vpn from local computer
- download and install openvpn in local computer
- import opvpn file in openvpn and connect
- connect ro rds db from mysql client. it works!
- disconnect vpn. reconnect rds db. it fails!

### Step 7: Create a seperate user other than admin
- connect rds db.
- run &#8595;
```
CREATE USER 'read-only-user' IDENTIFIED BY 'passwd';
GRANT SELECT, RELOAD, PROCESS, REFERENCES, INDEX, SHOW DATABASES, SHOW VIEW, TRIGGER ON *.* TO 'read-only-user'@'%' WITH GRANT OPTION;
```
Note: read-only-user can connect to db with vpn and have only read only permissions;
