## Creating Key Pair and using the Key to Create an EC2 Instance

### Task-1: Creating Key-Pair
```
vi provider.tf
```
```
provider "aws" {
  region = "us-east-1"
}
```
vi key.tf
```
```hcl
resource "aws_key_pair" "capstone-key" {
  key_name   = "capstone-key"
  public_key = tls_private_key.capstone_key_pair.public_key_openssh  #Passing the Public Key 
}

#Generating the Key pair
resource "tls_private_key" "capstone_key_pair" {
  algorithm = "RSA"
  rsa_bits  = 4096
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
