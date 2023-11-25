## Creating a MySQL Database with RDS 
```
cd /home/ubuntu
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/lab_12_rds.tar.gz
```
```
tar -zxvf lab_12_rds.tar.gz
```
```
cd lab_12_rds
```
```
ls
```
```
vi rds.tf
```
In `rds.tf` file in line-24 comment the line by adding #.
```
cat output.tf
```
```
vi vpc.tf
```
In `vpc.tf` file Add `#` in front of line 7, ie... `enable_classiclink = "false"` and replace your `AZs` for `3 Public Subnets` and `3 Private Subnets`
```
vi vars.tf
```
In `vars.tf` file replace your `region` and add your `region's AMI ID` in the list.
```
ssh-keygen -f mykey
ls
```
```
terraform init
```
```
terraform plan
```
`Note:` If you find any error on Line No-24 (`name = "mydb"`) just open file and Comment it by adding #.
```
terraform apply -auto-approve
```
```
ssh -i mykey -l ubuntu <PUBLIC IP>
```
```
sudo apt-get update
```
```
sudo apt-get install mysql-client
```
Get the endpoint from the AWS console. (do not include the port number while entering the endpoint)

**Example:** MySQL -u admin -h `mysql.cfkfveiseeie.us-east-2.rds.amazonaws.com` -p
```
mysql -u admin -h <endpoint> -p
```
`Note:` When it prompts for the password enter `password`

After LogingIn Use below Commands for DataBases
```
show databases;
```
#### Create a new database
```
create database demoDB;
```
```
show databases;
```
```
use demoDB
```
#### Use the `status` command to list the DB in which you are in.
```
status
```
#### Create table
```
create table customers(
   ID   INT              NOT NULL,
   NAME VARCHAR (20)     NOT NULL,
   AGE  INT              NOT NULL,
   ADDRESS  CHAR (25) ,
   SALARY   DECIMAL (18, 2),       
   PRIMARY KEY (ID)
);
```
#### Use the "desc" command to check if the table is created
```
desc customers;
```
Include some values in the DB:
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (1, 'Richard', 32, 'London', 50000.00 );
```
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (2, 'Rachel', 25, 'Paris', 45000.00 );
```
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (3, 'Mathew', 23, 'Brussels',55000.00 );
```
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (4, 'John', 25, 'Texas', 65000.00 );
```
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (5, 'Jane', 27, 'NewYork', 60000.00 );
```
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (6, 'Komal', 22, 'Bangalore', 45000.00 );
```
#### To view the table:
```
SELECT * FROM customers;
```
To view only selected attributes, run below command.
```
select NAME, AGE from customers;
```
#### To delete the table:
```
drop table customers;
```
* `Exit` from mysql client.
* `Exit` from the mysql `anchor EC2.`
```
terraform destroy
```
Once destroyed, Delete the directory and zip file as well.
```
cd ~
rm -rf lab_12_rds
```
```
rm -rf lab_12_rds.tar.gz
```
