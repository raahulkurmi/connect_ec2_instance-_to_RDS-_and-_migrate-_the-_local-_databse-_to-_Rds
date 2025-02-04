#first you need to install mariadb/anyother database in the ec2 machine
sudo su -
yum -y install mariadb-server wget
systemctl enable mariadb
systemctl start mariadb
yum -y update


#then you need to have same rds on the AWS
#connect both of the Databases using
mysql -h <endpoint of the Rds> -P 3306 -u admin  -p  #-u represnt the user of the RDS you created 
#after hitting enter it will ask for the password for the admin user you inserted while creating your RDS




# AWS RDS Masterclass Commands

## Databases on EC2 Instance - Demo
### Begin Configuration :
```bash
sudo su -
yum -y install mariadb-server wget
systemctl enable mariadb
systemctl start mariadb
yum -y update
```
### Set Environmental Variables
```bash
DBName=ec2db
DBPassword=admin123456
DBRootPassword=admin123456
DBUser=ec2dbuser
```
### Database Setup on EC2 Instance:
```bash
echo "CREATE DATABASE ${DBName};" >> /tmp/db.setup
echo "CREATE USER '${DBUser}' IDENTIFIED BY '${DBPassword}';" >> /tmp/db.setup
echo "GRANT ALL PRIVILEGES ON *.* TO '${DBUser}'@'%';" >> /tmp/db.setup
echo "FLUSH PRIVILEGES;" >> /tmp/db.setup
mysqladmin -u root password "${DBRootPassword}"
mysql -u root --password="${DBRootPassword}" < /tmp/db.setup
rm /tmp/db.setup
```
### Adding some dummy data to the Database inside EC2 Instance:
```bash
mysql -u root --password="${DBRootPassword}"
USE ec2db;
CREATE TABLE table1 (id INT, name VARCHAR(45));
INSERT INTO table1 VALUES(1, 'Virat'), (2, 'Sachin'), (3, 'Dhoni'), (4, 'ABD');
SELECT * FROM table1;
```
### Migration of Database in EC2 Instance to RDS Database:
```bash
mysqldump -u root -p ec2db > ec2db.sql
mysql -h <replace-rds-end-point-here> -P 3306 -u admin -p mydb < ec2db.sql
#here my user name was admin and databse name was my db do i gave the name accordingly you have to change as per your username of the aws database
mysql -h <replace-rds-end-point-here> -P 3306 -u admin -p
USE rdsdb
SELECT * FROM table1;
```



