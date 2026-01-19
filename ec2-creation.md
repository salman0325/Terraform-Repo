```terraform.tf
terraform.tf

terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "6.22.1"
    }
  }
}
```
```terraform.tf
provider.tf
provider "aws" {
    region = "ap-south-1"
  
}
```
```terraform.tf
#key pair (login)
resource "aws_key_pair" "key_pair" {
  key_name = "terrafom-key"
  public_key = file("/home/salman/.ssh/id_ed25519.pub")
}


#vpc & security group
resource "aws_default_vpc" "default" {
  
}


#security group 
resource "aws_security_group" "my_security_group" {
    name = "automate-sg"
    description = "this will and a tf genarated security group"
    vpc_id = aws_default_vpc.default.id # interpolation


    #inbound rules
    ingress{
        from_port = 22
        to_port = 22
        protocol = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
        description = "ssh-open"
    } 


    ingress {
        from_port = 80
        to_port = 80
        protocol = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
        description = "HTTP port open"
    }

    #outbount rules
    egress {
        from_port = 0
        to_port = 0
        protocol = "-1" #semantically equivalent to all ports
        cidr_blocks = ["0.0.0.0/0"]
        description = "all access open outbound"

    }


    tags ={
        Name = "automate-sg"
    }
  
}


#ec2 instance

resource "aws_instance" "my_instance" {
    key_name = aws_key_pair.key_pair.key_name
    security_groups = [aws_security_group.my_security_group.name]
    instance_type = "t3.micro"
    ami = "ami-02b8269d5e85954ef" #ubuntu
    root_block_device {
      volume_size = 8
      volume_type = "gp3"
    }
    tags = {
      Name = "terraform-automate"
    }
  
}
```

```terraform.tf
output.tf
#----------outptu for count-----------

# output "ec2_public_ip" {
#     value = aws_instance.my_instance[*].public_ip # * means multipel instance

# }
# output "ec2_public_dns" {
#   value = aws_instance.my_instance[*].public_dns #single output [*] its measn mutiple 
# }
# output "ec2_private_ip" {
#   value = aws_instance.my_instance[*].private_ip
# }

#-----------------output for each -----------
output "ec2_public_ip" {
  value = [
    for key in aws_instance.my_instance : key.public_ip
  ]

}
```

```terraform.tf
varable.tf
variable "ec2_instance_type" {
  default = "t3.micro"
  type    = string

}

# variable "ec2_root_storage_size" {
#   default = 8
#   type = number
# }

variable "ec2_default_root_storage_size" {
  default = 10
  type    = number

}


variable "ec2_ami_id" {
  default = "ami-02b8269d5e85954ef"
  type    = string

}
 
variable "env" {
  default = "prod"
  type = string
  
}
```
```terraform.tf
#key pair (login)
resource "aws_key_pair" "key_pair" {
  key_name   = "terrafom-key"
  public_key = file("/home/salman/.ssh/id_ed25519.pub")
}


#vpc & security group
resource "aws_default_vpc" "default" {

}


#security group 
resource "aws_security_group" "my_security_group" {
  name        = "automate-sg"
  description = "this will and a tf genarated security group"
  vpc_id      = aws_default_vpc.default.id # interpolation


  #inbound rules
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
    description = "ssh-open"
  }


  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
    description = "HTTP port open"
  }

  #outbount rules
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1" #semantically equivalent to all ports
    cidr_blocks = ["0.0.0.0/0"]
    description = "all access open outbound"

  }


  tags = {
    Name = "automate-sg"
  }

}


#ec2 instance

resource "aws_instance" "my_instance" {
  for_each = tomap({
    "tws-junoon-automate-micro" = "t3.micro",
    tws-junoon-automate-medium  = "t3.micro"

  }) #meta argument
  # count = 2 #meta argument

  depends_on = [aws_security_group.my_security_group,aws_key_pair.key_pair]
  key_name        = aws_key_pair.key_pair.key_name
  security_groups = [aws_security_group.my_security_group.name]
  # instance_type = var.ec2_instance_type
  instance_type = each.value
  ami           = var.ec2_ami_id #ubuntu

  user_data = file("install_nginx.sh")


  root_block_device {
    # volume_size = var.ec2_root_storage_size
    volume_size = var.env == "prod" ? 20 : var.ec2_default_root_storage_size
    volume_type = "gp3"
  }
  # tags = {
  #   Name = "terraform-automate"
  # }
  tags = {
    Name = each.key
  }

}
```
```terraform.tf


#!/bin/bash

# Update system packages
sudo apt-get update -y

# Install Nginx
sudo apt-get install nginx -y

# Start and enable Nginx service
sudo systemctl start nginx
sudo systemctl enable nginx

# Write custom HTML message
echo "hi its me" | sudo tee /var/www/html/index.html > /dev/null

```
```terraform.tf
resource "aws_instance" "my_instance" {
  instance_type = "unknown"
  ami = "unknown"
}

#terraform import aws_instance.my_instance imiid
```




  
