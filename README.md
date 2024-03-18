# Autoscaling-EC2-Web-Server
#### Autoscaling with EC2 Web Server give users a smoother experience when visiting their favorite website! With the Application Load Balancer and Autoscaling Group, traffic from users is automatically balanced even when traffic is heavy or light. Resources dynamically add or reduce capacity depending on traffic conditions—making maintenance easier while providing consistent performance. 


## Architectural Diagram
![AutoScaleArch](https://github.com/ericincloud/Autoscaling-EC2-Web-Server/assets/144301872/f214b5fd-1859-4190-a049-d3dc064728ac)

### NOTE: *Terraform file (main.tf) in repository.* 
#### Launch using Terraform requires only configuring the ALB's security group. Simply replace outbound HTTP Port 80 from `anywhere` to HTTP Port 80 `EC2 Instances's Private IPv4 address`. 


## Step 1: Create VPC  
#### Create VPC with 3 Public subnets and 3 Private subnets.
  
![CreateASvpc](https://github.com/ericincloud/Autoscaling-EC2-Web-Server/assets/144301872/5ee190ee-d547-4d5b-91bf-5a991bbdd4aa)

## Step 2: Create NGINX EC2
#### Create an EC2 instance installed with the Bitnmai NGINX AMI web server in a private subnet.
  
![bitnaminginx](https://github.com/ericincloud/Autoscaling-EC2-Web-Server/assets/144301872/9be911f0-3b13-4b8a-8820-aa416eabe595)
![nginxec2](https://github.com/ericincloud/Autoscaling-EC2-Web-Server/assets/144301872/0d110e4e-7116-43d6-b76a-e7260b9f2c3f)

## Step 3: Setup ALB 
#### Create target group containing the NGINX EC2 instance. Then setup an Application Load Balancer in public subnet and connect it to the NGINX web server EC2 instance. 

#### Edit the ALB's security group Inbound rule to allow `HTTP` traffic on `Port 80` from `0.0.0.0/0` anywhere + Allow Outbound traffic on Port 80 to the EC2 instance using it's private IPv4 address. 

#### In the EC2's security group: Add Inbound EC2 security group rule allowing HTTP Port 80 for the ALB's security group. Add Outbound EC2 security group rule allowing HTTP Port 80 for the ALB's security group. Verify functionality by accessing the ALB DNS name. A default NGINX webpage should show up. 
  
![defaultnginx](https://github.com/ericincloud/Autoscaling-EC2-Web-Server/assets/144301872/75414bd4-4f52-43f7-a3cb-ab04aded7263)

## Step 4: Create autoscaling group 
#### Create Launch template with the Bitnami NGINX AMI and in a private subnet. The configuration should be of the initial Bitnami NGINX EC2 instance. Then create autoscaling group that takes into account the ALB and EC2. 
  
![AStarget](https://github.com/ericincloud/Autoscaling-EC2-Web-Server/assets/144301872/ccff6e1f-950d-4f22-b805-892fa27c7f45)
![ASGroup](https://github.com/ericincloud/Autoscaling-EC2-Web-Server/assets/144301872/ef8a7ba7-78d6-486e-9b51-2652bd935917)

#### Create 2 simple scaling policies - one that adds capacity units/instances if CPU utilization is over 70% and another that removes capacity units/instances if CPU utilization is under 40%.
![add70](https://github.com/ericincloud/Autoscaling-EC2-Web-Server/assets/144301872/c4aee8ba-85a2-45f9-be56-ef74544b450d)
![ASpolicies](https://github.com/ericincloud/Autoscaling-EC2-Web-Server/assets/144301872/a62aee57-49b2-4ca3-a7a4-835f15a3560a)


## Step 5: Setup EventBridge scaling alarm 
#### Setup an CloudWatch Events scaling alarm that increases the number of EC2 instances available in the auto scaling group if CPU is over 70% and reduced the number of EC2 instances if under 40%. In CloudWatch, create an alarm > search for `CPUUtlization` > Select the newly EC2 CPUUtilization autoscaling group metric
  
![CWcpu](https://github.com/ericincloud/Autoscaling-EC2-Web-Server/assets/144301872/461a8956-0d08-425c-8105-b72e06c4ad93)

#### Create 2 alarms. One that adds instance when CPU utilization is over 70% and another that removes instance when CPU utilization is undert 40%.
![CW40and70](https://github.com/ericincloud/Autoscaling-EC2-Web-Server/assets/144301872/70ae1543-547d-41f9-a794-e241f1338937)

### Finish!

## Notes
* Remember to edit the ALB security group to allow `HTTP` traffic on `Port 80` from `0.0.0.0/0` anywhere.
* Use internet facing ALB.





