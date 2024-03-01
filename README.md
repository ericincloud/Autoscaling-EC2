# Autoscaling-EC2
#### 
#### 

## Architectural Diagram
![IMAGE]()

## Step 1: Create VPC  
#### Create VPC with 3 Public subnets and 3 Private subnets.
  
![IMAGE]()

## Step 2: Create NGINX EC2
#### Create an EC2 instance installed with the Bitnmai NGINX AMI web server in a private subnet.
  
![IMAGE]()

## Step 3: Setup ALB 
#### Create target group containing the NGINX EC2 instance. Then setup an Application Load Balancer in public subnet and connect it to the NGINX web server EC2 instance. Edit the ALB security group to allow `HTTP` traffic on `Port 80` from `0.0.0.0/0` anywhere. Verify functionality by accessing the ALB DNS name. A default NGINX webpage should show up. 
  
![IMAGE]()

## Step 4: Create autoscaling group 
#### Create Launch template with the Bitnami NGINX AMI and in a private subnet. The configuration should be of the initial Bitnami NGINX EC2 instance. Then create autoscaling group that takes into account the ALB and EC2. Create 2 simple scaling policies - one that adds capacity units/instances if CPU utilization is over 70% and another that removes capacity units/instances if CPU utilization is under 40%. 
  
![IMAGE]()

## Step 5: Setup EventBridge scaling alarm 
#### Setup an CloudWatch Events scaling alarm that increases the number of EC2 instances available in the auto scaling group if CPU is over 70% and reduced the number of EC2 instances if under 40%. In CloudWatch, create an alarm > search for `CPUUtlization` > Selecte the newly EC2 CPUUtilization autoscaling group metric
  
![IMAGE]()

Finish!

## Notes
*
*

## Reference
*
*




