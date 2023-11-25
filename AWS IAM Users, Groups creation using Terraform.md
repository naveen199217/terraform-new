## Creating IAM Users, Groups using Terraform.
```
cd /home/ubuntu
```
```
mkdir iam-users && cd iam-users
```
Create a new `iam.tf` file to define your IAM resources.
```
vi iam.tf
```
Copy and paste the below Code to create users and group also replace your `region`
```
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_group" "example_group" {
  name = "infosys"
}

resource "aws_iam_user" "example_user" {
  for_each = toset(["user1", "user2", "user3", "user4", "user5", "user6", "user7", "user8", "user9", "user10"])

  name = each.key
}

resource "aws_iam_user_group_membership" "example_membership" {
  for_each = aws_iam_user.example_user

  user   = each.value.name
  groups = [aws_iam_group.example_group.name]
}

```
```
terraform init
```
```
terraform fmt
```
```
terraform plan
```
```
terraform apply
```
Once applied, verify the IAM User and group in the console and then Destroy.
```
terraform destroy
```
Once destroyed, remove the Directory.
```
cd ~
rm -rf iam-users
```
#### =========================END of LAB-08=========================

## Lab-9: Creating AWS resources using terraform modules
```
cd /home/ubuntu/
```
```
sudo apt install tree -y
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/terraform-modules.tar.gz
```
```
tar -xvf terraform-modules.tar.gz
```
```
cd terraform-modules
```
```
tree
```
Cat all files to see the module structure
```
vi main.tf
```
Add the below code after block `module "my_security_group"`
```
output "secgrpid" {
  description = "Newly created sec grp"
  value       = module.my_security_group.sgid
}
```
```
cat provider.tf
```
**Note:** No change needed in `provider.tf`
```
vi variables.tf 
```
**Note:** Replace the `Region` Default `VPC ID,` `AMI Id` and `Subnet ID` from your Allocated region in `variable.tf` file.
* `Change vpc_id` to default VPC in your region (**Ex:** vpc-0e608033e14b01c3c)
* `Change subnet id` Use any available subnets from AZ `a or b`. (**Ex:** subnet-086dd80df2e64b56b)

Then, Save it

Now, Create a key pair. The same public key will be used in the new EC2 Instance.
```
ssh-keygen -f mykey
```
```
terraform init
```
```
terraform fmt
```
```
