```hcl
resource "aws_key_pair" "capstone-key" {
  key_name   = "capstone-key"
  public_key = tls_private_key.capstone_private_key.public_key_openssh  #Passing the Public Key 
}

#Generating the Key pair
resource "tls_private_key" "capstone_private_key" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

#Store the private Key on Local
resource "local_file" "mykey" {
  content = tls_private_key.capstone_private_key.private_key_pem
  filename = "mykey"
}
```
