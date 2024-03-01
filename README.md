# Autoscaling-EC2
#### 
#### 

## Architectural Diagram
![IMAGE]()

## Step 1: Create VPC  
#### Create VPC with 3 Public subnets and 3 Private subnets.
  
![IMAGE]()

## Step 2: Create NGINX EC2
#### Create an EC2 instance installed with the NGINX web server in a private subnet.
  
![IMAGE]()

## Step 3: Setup ALB 
#### Setup an Application Load Balancer in the public subnet and connect it to the NGINX web server EC2 instance.
  
![IMAGE]()

## Step 4: Create autoscaling group 
#### Create autoscaling group that takes into account the ALB and EC2
  
![IMAGE]()

## Step 5: Setup EventBridge scaling alarm 
#### Setup an EventBridge scaling alarm that increases the number of EC2 instances available in the auto scaling group if CPU is over 70% and reduced the number of EC2 instances if under 40%.
  
![IMAGE]()

## Notes
*
*

## Reference
*
*




