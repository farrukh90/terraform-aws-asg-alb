# AWS ASG with ALB and WAF Terraform module 

## The module creates:
- Autoscaling Group
- Application Load Balancer
- AWS WAF Classic with two blacklist rules (country name and IP address)  

VPC and subnets should be created prior to applying this module.
Script in userdata.sh file needs to be customised. 

### Copy this code into a .tf file and run terraform apply


```hcl
module "ASG-with-ALB" {
    source             = "osmdilya/asg-alb/aws"
    region             = "us-east-1"
    vpc_id             = "vpc-0234cd7ed377fefe9"
    subnet1            = "subnet-0125f46f438c78275"
    subnet2            = "subnet-017bfd666a439d8aa"
    key_name           = "jump_box"
    key_location       = "~/.ssh/id_rsa.pub"

    # Autoscaling parameters
    desired_capacity   = 2
    max_size           = 2
    min_size           = 2
    instance_type      = "c5.large"
    on_demand_base_capacity = 0
    on_demand_percentage_above_base_capacity = 25

    # Load Balancer parameters
    load_balancer_port = "80"
    load_balancer_protocol = "HTTP"
    lb_target_group_port = "80"
    lb_target_group_protocol = "HTTP"

    # Security group ingress from/to ports
    sec_group_ingress_from_port1 = "22"
    sec_group_ingress_to_port1 = "22"
    sec_group_ingress_from_port2 = "80"
    sec_group_ingress_to_port2 = "80"
    sec_group_ingress_from_port3 = "443"
    sec_group_ingress_to_port3 = "443"


    # WAF Classic parameters
    blacklisted_ip = "192.0.7.0/24"
    blacklisted_country = "CN"


    tags = {
    Name = "ASG-with-ALB"
                    
    }
}
```