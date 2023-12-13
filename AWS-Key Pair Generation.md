## Creating Key Pair and passing the key when Creating an EC2 Instance
```
mkdir capstone && cd capstone
```


### Task-1: Creating Key-Pair
```
vi provider.tf
```
```hcl
provider "aws" {
  region = "us-east-1"
}
```
```
vi key.tf
```
```hcl

#Generating the Key pair
resource "tls_private_key" "capstone_key_pair" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

#Storing the Public key in AWS
resource "aws_key_pair" "capstone-key" {
  key_name   = "capstone-key"
  public_key = tls_private_key.capstone_key_pair.public_key_openssh  #Passing the Public Key 
}


#Store the private Key on Local
resource "local_file" "mykey_private" {
  content = tls_private_key.capstone_key_pair.private_key_pem
  filename = "capstone-key"
}

resource "local_file" "mykey_public" {
  content = tls_private_key.capstone_key_pair.public_key_openssh
  filename = "capstone-key.pub"
}

```
### Task-2: Passing the key when Creating an EC2 Instance
```
vi instance.tf
```
```hcl
resource "aws_instance" "ec2" {
  instance_type = "t2.micro"
  ami = "ami-0fc5d935ebf8bc3bc"
  key_name = "capstone-key"
}
```
```
terraform init
```
```
terraform plan 
```
```
terraform apply -auto-approve
```
