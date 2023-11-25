## Launching Auto-Scaling services

### Task-1: Create ASG
```
cd /home/ubuntu/
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/lab_14_autoscaling.tar.gz
```
```
tar -zxvf lab_14_autoscaling.tar.gz
```
```
cd lab_14_autoscaling/
ls
```
```
cat autoscaling.tf
```
```
cat autoscalingpolicy.tf
```
```
vi vpc.tf
```
In `vpc.tf` file Add `#` in front of line 7, ie... `enable_classiclink = "false"` and replace your `AZs` for `3 public Subnets` and `3 Private Subnets`
```
vi vars.tf
```
In `vars.tf` file replace your `region` and Include your `region's AMI ID` in the list.
```
ssh-keygen -f mykey
ls
```
```
terraform init
```
```
terraform plan
```
```
terraform apply 
```
Now, Check In the Console that the resources are created 

  1. `EC2 dashboard` >> `Auto Scaling Groups` >> `example-autoscaling`
  2. `Amazon Cloud Watch` >> `Alarms`
  3. `VPC,` `Subnets,` and `RT` etc...


### Task-2: Increase CPU utilization in your new ec2 instance.

Now, let's connect with the EC2 created by ASG.

```
ssh -i mykey -l ubuntu <Public IP> 
```
```
sudo apt-get update
```
```
sudo apt-get install stress
```
```
stress --cpu 3 -v --timeout 300
```
* Now go back to `AWS console` > `EC2 Dashboard` > `Instances` then select the New Instance Created by `ASG` > `Description` and click on `Monitoring.` Now you see High CPU utilization

  (Or)
  
* Go to `AWS Console` > `CloudWatch` > `Alarms` and observe the alarm states and CPU Utilization.
* To check whether the `New Instance` is added, Go to `AWS Console` > `EC2` > `Instances.` You can see that one more instance is created with the same name because of the high CPU.  

Once done `Exit` from EC2 Instance
```
terraform destroy
```
Once done, remove the Directory and Zip file.
```
cd ~
rm -rf lab_14_autoscaling
```
```
rm -rf lab_14_autoscaling.tar.gz
