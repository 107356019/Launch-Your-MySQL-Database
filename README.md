# Launch Your MySQL Database

## Scenario
This lab builds on the [previous](https://github.com/ecloudvalley/Launch-an-Amazon-EC2-in-LAMP-Environment) lab. It walks you through launching an Amazon Relation Database Service ([Amazon RDS](https://aws.amazon.com/rds/)) DB instance. You will configure the LAMP server that you previously created to use Amazon RDS for its relational database management system needs. This lab is designed to connect DB instance through a web service phpMyAdmin.

You will build the following architecture:

![1.png](/images/1.png)

## Prerequisites
>The workshop’s region will be in ‘N.Virginia’

>Download Putty and PuTTYgen: IF you don’t already have the **PuTTy client/PuTTYgen** installed on your machine, you can download and then launch it from here: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

## Lab tutorial
### Create a VPC security group for the RDS DB Instance
1.1.     In the AWS Management Console, on the **service** menu, click **VPC**.

1.2.     In the navigation, click **Security Groups**.

1.3.     Click **Create Security Group**.

1.4.     In the Create Security Group dialog box, enter the following details:
* Name tag: DBSecurityGroup
* Group name: DBSecurityGroup
* Description: DB instance security group
* VPC: Click My Lab VPC

1.5.     Click **Yes, Create**.

1.6.     Select **DBSecurityGroup** you just created.

1.7.     Click the **Inbound Rules** tab, and then click **Edit**.

1.8.     Create an inbound rule with the following details:
* Type: MySQL/Aurora(3306)
* Protocol: TCP(6)
* Source: Click LAMPSecurityGroup 
>Search sg-xxxxxxxx to find LAMPSecurityGroup

1.9.     Click **Save**.

### Create Private Subnets for Your Amazon RDS Instances

2.1.     In the navigation pane, click **Subnets**.

2.2.     Select **Public Subnet 1**, scroll down to the Summary tab in the lower pane. Take note of the **Availability Zone** for this subnet.

2.3.     Select **Public Subnet 2**, scroll down to the Summary tab in the lower pane. Take note of the **Availability Zone** for this subnet.

2.4.     Click Create Subnet dialog box, enter the following details:
* Name tag: **Private Subnet 3**
* VPC: **My Lab VPC**
* Availability Zone: **us-east-2a**
* CIDR block: **10.0.5.0/24**

2.5.     Click **Yes, Create**.

2.6.     Click **Create Subnet**.

2.7.     In Create Subnet dialog box, enter the following details:
* Name tag: **Private Subnet 4**
* VPC: **My Lab VPC**
* Availability Zone: **us-east-2b**
* CIDR block: **10.0.6.0/24**

2.8.     Click **Yes, Create**.

### Create a DB Subnet Group

3.1.     In the **AWS Management Console**, on the **service** menu, click **RDS**.

3.2.     In the navigation pane, click **Subnet Groups**.

3.3.     Click **Create DB Subnet Group**.

3.4.     On the Create DB Subnet Group page, enter the following details:
* Name: **dbsubnetgroup**
* Description: **Lab DB Subnet Group**
* VPC ID: Click **My Lab VPC**

3.5.     For **Availability Zone**, choose **us-east-2a**, choose **10.0.5.0/24**, then click **Add**.

3.6.     Choose another Availability Zone as **us-east-2b**, choose **10.0.6.0/24**, then click **Add**.

3.7.     Click **Create**.

3.8.     If you do not see your new subnet group, click the refresh icon in the upper-right corner of the console.

### To launch a MySQL DB instance

4.1.     In the **AWS Management Console**, on the **service** menu, click **RDS**.

4.2.     In the navigation pane, choose **Instances**.

4.3.     Choose **Launch DB Instance** to start the **Launch DB Instance Wizard**. The wizard opens on the **Select Engine** page.

![2.png](/images/2.png)

4.4.     In the **Select Engine** window, click the **Next** button for the MySQL DB engine.

4.5.     Click **Use case** step. Click **Production - MySQL**, Click **Next**.

4.6.     On the **Specify DB Details** page, enter the following details:
* DB Instance Class: Choose db.t2.micro—1 vCPU, 1GiB RAM.
* Multi-AZ Deployment: Click Yes.
* DB Instance Identifier: LabDBInstance
* Master Username: labuser
* Master Password: labpassword
* Confirm Password: labpassword

4.7.     Click **Next**.

4.8.     On the **Configure Advanced Settings** page, enter the following details and leave all other values with their default:
* VPC: My Lab VPC
* Subnet Group: dbsubnetgroup
* Publicly Accessible: No
* VPC Security Group(s): DBSecurityGroup
* Database Name: sampled
* Encryption: **Disable Encryption**
* Backup: Backup retention period: **0 days**
* Monitoring: **Disable enabled monitoring**
* Maintenance: choose **Disable auto minor version upgrade**

4.9.     Click **Launch DB Instance**.

4.10.     Click **View DB Instances details**.

4.11.     Select **labdbinstance** and scroll down to **Details** wait until **Endpoint** is available or modifying – this may take up to 10 minutes. Use the refresh icon in the top right corner to check for updates.

![3.png](/images/3.png)

4.12.     Copy and save the **Endpoint**, making sure to not copy the: 3306 – your **Endpoint**. **Endpoint** should look similar to the following as below example: db.choi5coyenv6.us-east-2.rds.amazonaws.com. You will change localhost to this endpoint later.

![4.png](/images/4.png)

### Use EC2 to Connect with Database

5.1.     Log in to your **LAMP Server** instance using SSH.

5.2.     Install MySQL tool.

    [ec2-user ~]$ sudo yum install mysql

5.3.     After check state, please key y to start install.
    
    Is this ok? [y/d/N]: y

5.4.     After installed MySQL, to log-in RDS server, key **‘mysql –h [Endpoint] –u [Username] -p’**, and press enter to key **[password]**.

5.5.     If you log-in to RDS, you can see the **mysql> select database();** and you can use the database.

5.6.     If you want to leave the RDS, please press **Ctrl+C** to exit.

## Conclusion

Congratulations! You now have learned how to:
* Setup network security included subnets and security group for the database.
* Create an Amazon Relation Database (Amazon RDS).
* Logged into your DB instance.




