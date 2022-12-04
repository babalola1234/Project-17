# Project-17
AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM. PART 2

## Creating Certificates from AWS before ALB and Auto Scalling Group etc

- Create ALB before Autoscalling Group
- Create Autoscalling Group ( Which needs the ALB)
- Target group listens on port 443 ( thats why we need certs)
- create listeners for target group
- create the launch template


- Did you notice how we have used `format()` function to dynamically generate a unique name for this resource? The first part of the `%s` takes the interpolated value of `aws_vpc.main.id` while the second` %s` appends a literal string `IG` and finally an exclamation mark is added in the end.

- If any of the resources being created is either using the `count `function, or creating multiple resources using a loop, then a key-value pair that needs to be unique must be handled differently.

- For example, each of our subnets should have a unique name in the tag section. Without the `format()` function, we would not be able to see uniqueness. With the `format` function

### IMPORTANT NOTE: We used the aws_security_group_rule to refrence another security group in a security group.




# Resources to be created As per our architecture we need to do the following:
### To create all the Networking resourses to set up AWS for the infrastructure using Terraform!
-  main vpc
- 2 Public subnets
- 4 Private subnets
- 1 Internet Gateway
- 1 NAT Gateway
- 1 EIP
- 2 Route tables (aws routes)

### To create all the Compute and Access Control configuration automation using Terraform!

- create IaM and Roles
- Create IAM policy for this role, Attach the Policy to the IAM Role
- Create Security Groups
- Create Target Group for Nginx, WordPress and Tooling
- Create certificate from AWS certificate manager
- Create an External Application Load Balancer and Internal Application Load Balancer.
- create launch template for Bastion, Tooling, Nginx and WordPress
- Create an Auto Scaling Group (ASG) for Bastion, Tooling, Nginx and WordPress
- Create Elastic Filesystem
- Create Relational Database (RDS)

## A little bit more about Tagging
- Tagging is a straightforward, but a very powerful concept that helps you manage your resources much more efficiently:

- Resources are much better organized in ‘virtual’ groups
- They can be easily filtered and searched from console or programmatically
- Billing team can easily generate reports and determine how much each part of infrastructure costs how much (by department, by type, by environment, etc.)
- You can easily determine resources that are not being used and take actions accordingly
- If there are different teams in the organisation using the same account, tagging can help differentiate who owns which resources.


- Now you can tag all you resources using the format below

```
  tags = merge(
    var.tags,
    {
      Name = format("%s-PublicSubnet-%s", var.name, count.index)
    },
  )
}
  ``` 

## Note: You can add multiple tags as a default set. for example, in our terraform.tfvars file we can have default tags defined.

```
tags = {
  Enviroment      = "production" 
  Owner-Email     = "babaloladeen1@gmail.com"
  Managed-By      = "Terraform"
  Billing-Account = "xxxxxxxxxxxx"
}
 ``` 

 ### Useful Terraform Documentation, go through this documentation and understand the arguement needed for each resources:

- ALB
- ALB-target
- ALB-listener

### Useful Terraform Documentation, go through this documentation and understand the arguement needed for each resources:

1. SNS-topic
2. SNS-notification
3. Austoscaling
4. Launch-template

- Useful Terraform Documentation, go through this documentation and understand the arguement needed for each resources:

1. RDS
2. EFS
3. KMS


## Before Applying, if you take note, we gave refrence to a lot of varibales in our resources that has not been declared in the `variables.tf` file. Go through the entire code and spot this variables and declare them in the variables.tf file.

- If you have done that well, you file should like this one below.
``` 
variable "region" {
  type = string
  description = "The region to deploy resources"
}

variable "vpc_cidr" {
  type = string
  description = "The VPC cidr"
}

variable "enable_dns_support" {
  type = bool
}

variable "enable_dns_hostnames" {
  dtype = bool
}

variable "enable_classiclink" {
  type = bool
}

variable "enable_classiclink_dns_support" {
  type = bool
}

variable "preferred_number_of_public_subnets" {
  type        = number
  description = "Number of public subnets"
}

variable "preferred_number_of_private_subnets" {
  type        = number
  description = "Number of private subnets"
}

variable "name" {
  type    = string
  default = "ACS"

}

variable "tags" {
  description = "A mapping of tags to assign to all resources."
  type        = map(string)
  default     = {}
}

variable "ami" {
  type        = string
  description = "AMI ID for the launch template"
}

variable "keypair" {
  type        = string
  description = "key pair for the instances"
}

variable "account_no" {
  type        = number
  description = "the account number"
}

variable "master-username" {
  type        = string
  description = "RDS admin username"
}

variable "master-password" {
  type        = string
  description = "RDS master password"
}

```



Additional tasks
In addition to regular project submission include following:

Summarise your understanding on Networking concepts like IP Address, Subnets, CIDR Notation, IP Routing, Internet Gateways, NAT
Summarise your understanding of the OSI Model, TCP/IP suite and how they are connected – research beyond the provided articles, watch different YouTube videos to fully understand the concept around OSI and how it is related to the Internet and end-to-end Web Solutions. You don not need to memorise the layers – just understand the idea around it.
Explain the difference between assume role policy and role policy
Congratulations!
Now you have fully automated creation of AWS Infrastructure for 2 websites with Terraform. In the next project we will further enhance our codes by refactoring and introducing more exciting Terraform concepts! Go ahead and continue your PBL journey with us!
