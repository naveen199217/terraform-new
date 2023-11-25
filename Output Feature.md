## Using Output Feature 

### Task-1: Using output feature of Terraform to get the IP Address of EC2 Instance
```
cd /home/ubuntu/
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/output-variable-lab-v0.13.5.tar.gz
```
```
tar -xvf output-variable-lab-v0.13.5.tar.gz
```
```
cd output-variable-lab/
ls
```
```
cat instance.tf
```
```
cat output.tf
```
```
cat vars.tf
```
In `vars.tf` file delete all the lines and add the below code. Also, ensure to replace your `region` and Include your `region's Ubuntu AMI ID` to the list.
```
vi vars.tf
```
```
variable "AWS_REGION" {
  default = "us-east-1"
}

variable "AMIS" {
  type = map(string)
  default = {
    us-east-2 = "ami-089c26792dcb1fbd4"
    us-west-2 = "ami-00448a337adc93c05"
    eu-west-1 = "ami-0e309a5f3a6dd97ea"
  }
}
```
Once replaced, save the changes.
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
terraform apply
```
```
ls 
cat private_ips.txt 
```
```
terraform output Public_ip
```
```
terraform output Private_ip
```
Use the `terraform destroy` command to clean the infrastructure used in this lab.
```
terraform destroy
```
Once done, remove the directory and Zip file using the `rm -rf` command below.
```
cd ~
rm -rf output-variable-lab
```
```
rm -rf output-variable-lab-v0.13.5.tar.gz
```

