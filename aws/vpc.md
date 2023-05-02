# Setup a secure vpc for application and database deployment

### Step 1: Create a vpc
- name: `myvpc`
- cidr: `10.0.0.0/16`

### Step 2: Create public and private subnets
- vpc_id: `myvpc`
- name: `pubsn`
- cidr: `10.0.1.0/24`
    #### add another sn
- name: `prisn`
- cidr: `10.0.2.0/24`

Note: two subnets ip range must not overlap with each other.

### Step 3: Create Internet Gateway
- name: `myigw`
- attach: `myvpc`

Note: This will allow public subnet to have internet access (incoming and outgoing).

### Step 4: Create Route Table
- name: `pubrt`
- add route: `0.0.0.0/0    myigw`
- subnet association: `pubsn`
#### add another rt
- name: `prirt`
- subnet association: `prisn`

### Step 5: Spin EC2 machines
- name: `app`
- ami: `amazon linux`
- instance: `t2.micro`
- network: `myvpc`
- subnet: `pubsn`
- public ipv4 address: `enable`
- user data: `sudo yum update && sudo yum install neofetch`
#### add another ec2
- name: `db`
- ami: `amazon linux`
- instance: `t2.micro`
- network: `myvpc`
- subnet: `prisn`
- public ipv4 address: `enable`
- user data: `sudo yum update && sudo yum install neofetch`

Note: 
