## Understanding local values, functions and data sources

### Task 1: Local Values 
```
mkdir lab4 && cd lab4
```
```
mkdir task1 && cd task1
```
```
vi local.tf
```
Add the given lines, by pressing "INSERT" and replace "ENTER THE AMI VALUE" with actual values
```
provider "aws" {
  region     = "us-east-1"
}

locals {
  custom_tags = {
    Team = "DevOps"
    Company = "CloudThat"
  }
}
resource "aws_instance" "instance1" {
   ami = "******ENTER THE AMI VALUE********"
   instance_type = "t2.micro"
   tags = local.custom_tags
}

resource "aws_instance" "instance2" {
   ami = "******ENTER THE AMI VALUE********"
   instance_type = "t2.small"
   tags = local.custom_tags
}

resource "aws_ebs_volume" "db_ebs" {
  availability_zone = "us-east-1a"
  size              = 8
  tags = local.custom_tags
}
```

Save the file using "ESCAPE + :wq!"
```
terraform init
```
```
terraform plan
```
```
terraform apply
```
```
terraform destroy
```

### Task 2: Functions
```
cd ..
```
```
mkdir task2 && cd task2
```
```
vi functions.tf
```
Add the given lines, by pressing "INSERT" 
```
provider "aws" {
  region     = var.region
}

variable "region" {
  default = "ap-south-1"
}

variable "tags" {
  type = list
  default = ["ec2-1","ec2-2"]
}

variable "ami" {
  type = map
  default = {
    "us-east-1" = "ami-0fc5d935ebf8bc3bc"         ******ENTER THE AMI VALUE********
    "us-west-1" = "ami-060d3509162bcc386"         ******ENTER THE AMI VALUE********
    "ap-south-1" = "ami-09ba48996007c8b50"        ******ENTER THE AMI VALUE********
  }
}

resource "aws_instance" "application-servers" {
   ami = lookup(var.ami,var.region)
   instance_type = "t2.micro"
   count = 2

   tags = {
     Name = element(var.tags,count.index)
   }
}
```

Save the file using "ESCAPE + :wq!"
```
terraform init
```
```
terraform plan
```
```
terraform apply
```
```
terraform destroy
```

### Task 3: Data sources

```
cd ..
```
```
mkdir task3 && cd task3
```
```
vi data.tf
```

Add the given lines, by pressing "INSERT" 
```
provider "aws" {
  region     = "us-east-2"
}

data "aws_ami" "ami" {
  most_recent = true
  owners = ["amazon"]


  filter {
    name   = "name"
    values = ["amzn2-ami-hvm*"]
  }
}

resource "aws_instance" "ec2instance" {
    ami = data.aws_ami.ami.id
   instance_type = "t2.micro"
}
```

Save the file using "ESCAPE + :wq!"

```
terraform init
```
```
terraform plan
```
```
terraform apply
```
```
terraform destroy
```
```
cd ~
rm -rf lab4
```
