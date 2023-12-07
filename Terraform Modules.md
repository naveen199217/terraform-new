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
