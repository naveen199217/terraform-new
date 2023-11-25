## Remote State using Amazon Simple Storage Service 

### Task-1: Manually Create a S3 Bucket using AWS Console 

* Create a new S3 bucket in your Allocated Region by name: `youryame-terraform` (S3 allows lowercase only)
* While creating,
    - Select `"ACLs enabled"`
    - Uncheck `"block public access"` and select `"I acknowledge that the current settings might result in this bucket and the objects within becoming public"`
    - `"Enable versioning"`
    - Then, click on `Create bucket`
* Once done, To cross-check whether the Bucket is created or not, run the below command in CLI.
```
aws s3 ls 
```
### Task-2: Configure Remote State
```
cd ~
mkdir S3-Lab && cd S3-Lab
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/remote_state_lab.tar.gz
```
```
tar -zxvf remote_state_lab.tar.gz
```
```
cd remote-state-lab
```
```
ls
```
```
cat instance.tf
```
```
cat vars.tf
```
```
vi vars.tf
```
In `vars.tf` file ensure to replace your `region` and Include your `region's Ubuntu AMI ID` to the list.

Once done, save the file and follow further steps.
```
cat provider.tf
```
Now, Create a New Configuration File for storing "`terraform.tfstate`" file in the backend. (ie. `Amazon S3.`)

```
vi backend.tf
```
Add the given lines, by pressing "INSERT" and add your `bucket's region` and `Bucket Name`
```
terraform {
  backend "s3" {
    region = "<Replace your s3 bucket region>"
    bucket = "<Replace your s3 bucket name>"
    key    = "terraform/remotestate"
  }
}
```
Save the file using "ESCAPE + :wq!"
```
cat backend.tf
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
terraform apply
```
* Go to the S3 bucket and click on `terraform` > `remotestate` > In Properties Copy the `Object URL` and paste it in Browser.
  (By default it shows Access Denied)
* To view the content of the file, in S3 Bucket tab, Click on `permission` and click on `Edit` under `Access control list (ACL)` > `Everyone (public access)` > Check `"Read"` then check `I understand the effects of these changes on this object` and then Click on `Save changes`
* Refresh the Object URL Page in the browser (or again Copy-paste the `object URL` into the web browser).
* Now, You should be able to access the state file and View the resources.
  (It shows the attributes of a single resource in the Terraform state of `aws_instance.terraform-remoteState`.)

Use the `terraform destroy` command to clean the infrastructure used in this lab, 
```
terraform destroy
```
Once done, Remove the directory and Zip file using "`rm -rf`"
```
cd ~
rm -rf S3-Lab
```
**Note:** Also Ensure to delete the `S3 Bucket` (To delete, first empty the Bucket and then Delete it.)
