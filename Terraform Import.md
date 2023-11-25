## Understanding Terraform Import
```
mkdir import && cd import
```
```
vi import.tf
```
Add the given lines, by pressing "INSERT" 
```
provider "aws" {
  region = "us-east-1"
}   

#terraform import aws_instance.test_instance i-0c94695f4d493269b
resource "aws_instance" "test_instance" {
  ami = "ami-0fc5d935ebf8bc3bc"
  instance_type = "t2.micro"
  tags = {
    name = "test_instance"
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
