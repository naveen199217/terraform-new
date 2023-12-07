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
