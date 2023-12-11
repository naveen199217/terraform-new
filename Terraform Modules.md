## Terraform Modules: Simplifying Infrastructure as Code

Terraform modules are a fundamental concept in Terraform, a widely-used Infrastructure as Code (IaC) tool. They play a crucial role in making infrastructure code more modular, manageable, and reusable. By encapsulating Terraform configurations, modules abstract and organize resources, making it easier to provision, manage, and scale infrastructure across various environments and projects.

**Creating a Terraform Module**

Let's walk through the process of creating a Terraform module for setting up an AWS S3 bucket.

**1. Directory Structure:**

Begin by structuring your module's files within a directory, which you can name, for example, `s3_bucket`.

```
s3_bucket/
├── main.tf
├── variables.tf
└── outputs.tf
```

**2. Defining Input Variables (`variables.tf`):**

Declare input variables that users of your module can customize when they include it in their configuration.

```hcl
# variables.tf
variable "bucket_name" {
  description = "The name of the S3 bucket"
  type        = string
}

variable "region" {
  description = "The AWS region for the S3 bucket"
  type        = string
  default     = "us-east-1"
}
```

**3. Resource Definition (`main.tf`):**

Write the primary configuration for the AWS S3 bucket resource, referencing the input variables declared earlier.

```hcl
# main.tf
provider "aws" {
  region = var.region
}

resource "aws_s3_bucket" "example" {
  bucket = var.bucket_name
  acl    = "private"
}
```

**4. Defining Output Values (`outputs.tf`):**

Specify any output values that users might want to access after using the module.

```hcl
# outputs.tf
output "bucket_id" {
  description = "The ID of the created S3 bucket"
  value       = aws_s3_bucket.example.id
}
```

**5. Using the Module in a Configuration:**

With your module defined, you can now use it in a Terraform configuration file. Create a separate file (e.g., `main.tf`) to use the module:

```hcl
# main.tf
module "my_s3_bucket" {
  source      = "./s3_bucket" # Use a relative path to your module directory.
  bucket_name = "my-unique-bucket-name"
  region      = "us-west-2"
}

output "my_s3_bucket_id" {
  description = "The ID of the created S3 bucket"
  value       = module.my_s3_bucket.bucket_id
}
```

In this example, you've instantiated the `s3_bucket` module, passing values for the `bucket_name` and `region` input variables while also accessing the `bucket_id` output value from the module.

**Conclusion**

Terraform modules are a powerful way to enhance code reusability, streamline infrastructure provisioning, and maintain well-organized infrastructure as code projects. They shine in scenarios with numerous resources and configurations, offering a structured approach to managing infrastructure.


## Task 1 : Creating AWS resources using terraform modules
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
mkdir terraform-modules
cd terraform-modules
```
```
mkdir modules
cd modules
```
```
mkdir ec2  key-pair  security-grp
```
```
cd ec2
```
```
vi instance.tf
```
```hcl
resource "aws_instance" "example" {
    ami = var.ami_id
    instance_type = var.instance_type
    subnet_id = var.subnet_id
    vpc_security_group_ids = [var.security_group]
    key_name = var.key_name

    tags = {
      Name = "Terraform-Created"
    }
    
}

output "ec2_id" {
  value = aws_instance.example.id
}
```
```
vi vars.tf 
```
```hcl

variable "aws_region"{
}

variable "ami_id"{
}

variable "instance_type"{
}

variable "subnet_id"{
}

variable "security_group"{
}

variable "key_name"{
}
```
```
cd ..
cd key-pair/
```
```
vi key.tf
```
```hcl
resource "aws_key_pair" "mykeypair" {
    key_name = var.key_name
    public_key =  file(var.PATH_TO_PUBLIC_KEY)
}


output "key-name"{
value = aws_key_pair.mykeypair.key_name

}
```
```
vi vars.tf
```
```hcl
variable "PATH_TO_PUBLIC_KEY" {
} 

variable "key_name" {
}
```
```
cd ..
cd security-grp
```
```
vi sg.tf
```
```hcl

provider "aws" {
  region = var.aws_region
}

resource "aws_security_group" "allow-ssh" {
    vpc_id = var.vpc_id
    name = var.sg_name
    description = "Security group that allows ssh and all egress traffic"
    egress {
        from_port = 0
        to_port = 0
        protocol = "-1"
        cidr_blocks = ["0.0.0.0/0"]
    }
    ingress {
        from_port = var.from_port
        to_port = var.to_port
        protocol = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }
    ingress {
        from_port = var.from_port2
        to_port = var.to_port2
        protocol = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }
    tags = {
        Name = "allow-ssh"
    }
}

output "sgid" {
  value = aws_security_group.allow-ssh.id
}

```
```
vi vars.tf
```
```hcl
variable "aws_region" {
}

variable "vpc_id" {
}

variable "from_port" {
}

variable "to_port" {
}

variable "from_port2" {
}

variable "to_port2" {
}

variable "sg_name" {
}

```
```
cd ../..
tree
```
Now, Create a key pair. The same public key will be used in the new EC2 Instance.
```
ssh-keygen -f mykey
```
```
vi main.tf
```
```hcl
# TO Create Security Group

module "my_security_group" {
    source ="./modules/security-grp"
    aws_region = var.region
    vpc_id = var.sg_vpcid
    from_port = var.from_port
    to_port = var.to_port
    from_port2 = var.from_port2
    to_port2 = var.to_port2
    sg_name = var.sg_name
}

# TO Create Key-Pair

module "my_key" {
    key_name = var.key_name
    source = "./modules/key-pair"
    PATH_TO_PUBLIC_KEY = var.public_key
}

# TO Create EC2 Instance

module "my_ec2" {
    source = "./modules/ec2"
    aws_region = var.region
    ami_id = var.ami_id
    instance_type = var.ins_type
    subnet_id = var.sub_id
    security_group = module.my_security_group.sgid
    key_name = module.my_key.key-name

}

output "secgrpid" {
  description = "Newly created sec grp"
  value       = module.my_security_group.sgid
}
```
```
tree
```
```
vi provider.tf
```
**Note:** No change needed in `provider.tf`
```hcl
provider "aws" {
  region = var.region
}
```
```
vi variables.tf 
```
**Note:** Replace the `Region` Default `VPC ID,` `AMI Id` and `Subnet ID` from your Allocated region in `variable.tf` file.
* `Change vpc_id` to default VPC in your region (**Ex:** vpc-0e608033e14b01c3c)
* `Change subnet id` Use any available subnets from AZ `a or b`. (**Ex:** subnet-086dd80df2e64b56b)

Then, Save it
```hcl

variable "region" {
    default = "us-east-1"
} 

variable "sg_vpcid" {
    default = "vpc-0145545ee19d71389"
}

variable "from_port" {
    default = 22
}

variable "to_port" {
    default = 22
}

variable  "sg_name" {
    default = "terraform-sgp"
}

variable "ami_id" {
    default = "ami-0fc5d935ebf8bc3bc"
}

variable "ins_type" {
    default = "t2.micro"
}

variable sub_id {
    default = "subnet-0881026ede3d83d8f"
}

variable key_name {
    default = "my-key-pair"
}

variable from_port2 {
    default = 80
}

variable to_port2 {
    default = 80
}

variable public_key {
    default = "mykey.pub"
}
```
```
terraform init
```
```
terraform fmt
```
```
terraform validate
```
```
terraform plan
```
```
terraform apply -auto-approve
```
```
terraform destroy -auto-approve
```
```
cd ..
rm -rf terraform-modules
```
