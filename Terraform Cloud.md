## Terraform Cloud

### Task 1: Create a Terraform Cloud Account

### Task 2: Create a new repo in Github

* Sign in in Github

* Click on New
In the repository name: Enter "Terraform-Cloud"
Click on Private
Click on Create Repository
Click on creating a new file
Name your file vars.tf
Add the following code
variable "AWS_ACCESS_KEY"{} 
variable "AWS_SECRET_KEY"{} 
Click on commit new file.
Add another file by clicking on add file dropdown and select create new file. 
Name the file as instance.tf, Insert the below contents and commit the file. 

provider "aws" { 
region = "sa-east-1" 
access_key=var.AWS_ACCESS_KEY 
secret_key=var.AWS_SECRET_KEY 
} 

resource "aws_instance" "web" { 
ami           = "ami-02dc8ad50da58fffd" 
instance_type = "t3.micro" 
tags = { 
Name = "HelloWorld" 
} 
}  

Task 3: Create a new workspace
=============================================================================
Click on create a new workspace
In tab 1, Select VCS (Version Control System) 
In tab 2, select Github
Once you click Github.com, a new window will popup. Sign into your GitHub account from there. Perform the verification and install Terraform on Git. 
Now, we have the GitHub account connected with Terraform cloud. In tab 3 choose the repository where Terraform configuration are present, in this case its â€œTerraform-Cloudâ€.
In tab 4, Provide the name for the workspace of  your own choice and click theâ€¯Create Workspaceâ€¯button. Once the workspace is created, you will see a success message in a popup. 

Task 4: Plan and Apply the changes 
=============================================================================
Now on the terraform cloud graphics, clickâ€¯Configure variables. In this demo, we will pass the credentials of AWS (Access key and secret key) to authenticate with users. 
Click on add variable and provide following details. Make sure you enable sensitive check box. 

AWS_ACCESS_KEY = Your aws access key 

AWS_SECRET_ACCESS_KEY = Your aws secret key 

Click onâ€¯Actionsâ€¯and Start new run and choose the option plan and apply
Once Plan is successful, scroll down a bit, and it will wait for the confirmation/approval to apply the changes. Clickâ€¯Confirm & Apply 
Provide a message in the textbox and click on Confirm Plan 
You will see that terraform apply is happening. 
Verify that the resource has been created in your AWS Console 

Task 5: Terminate the resources 
====================================================================================
On the settings tab, click on Destruction and Deletion 
Now, click onâ€¯Create Destroyâ€¯Plan. 
Provide the Workspace name and click on Queue Destroy plan 
Now you will notice that queue is scheduled. The deletion queue has two stages. Plan and apply (to delete the infra) 
Approve to start the delete operation by clicking Confirm & Apply
Once completed, check the run tab to check the status. 
Check the AWS console to verify that the resources are no longer active. 