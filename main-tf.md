.terraform/infa-app/
```1.tf
resource "aws_s3_bucket" "romote_s3" {
    bucket = "${var.env}-${var.bucket_name}"
    tags = {
      Name = "${var.env}-${var.bucket_name}"
      Environment = var.endv
      
    }
  
}
```
```2.tf
resource "aws_dynamodb_table" "basic_dynamodb_table" {
  name         = "${var.env}-infra-app-table"
  billing_mode = "PAY_PER_REQUEST"

  hash_key     = var.hash_key

  attribute {
    name = "LockID"
    type = "S"     # S = String, N = Number, B = Binary
  }

  tags = {
    Name = "${var.env}-infra-app-tables"
    Environment = var.env

  }
}
```
```3.tf
variable "env" {
    description = "this is my  envirmant for my infra"
    type = string
  
}
variable "bucket_name" {
    default = "dev"
    description = "this is the bucket name"
    type = string
  
}
variable "instance_count" {
    description = "this is the number of ec2 instance"
    type = number
  
}
variable "instance_type" {
  
  description = "this is the instance type"
  type = string

}
variable "ec2_ami_id" {
    description = "this is the ami id"
    type = string
  
}
variable "hash_key" {
    description = "this is the hash key for dynamo db infra"
  
}
```
```4.tf
#key pair (login)
resource "aws_key_pair" "key_pair" {
  key_name   = "${var.env}-infra-app-key"
  public_key = file("/home/salman/.ssh/id_ed25519.pub")
  tags = {
    Environment = var.env
  }
}


#vpc & security group
resource "aws_default_vpc" "default" {

}


#security group 
resource "aws_security_group" "my_security_group" {
  name        = "${var.env}-infra-app-sg"
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
    Name = "${var.env}-infra-app-sg"
  }

}


#ec2 instance

resource "aws_instance" "my_instance" {
    count = var.instance_count
   #meta argument
  # count = 2 #meta argument

  depends_on = [aws_security_group.my_security_group,aws_key_pair.key_pair]
  key_name        = aws_key_pair.key_pair.key_name
  security_groups = [aws_security_group.my_security_group.name]
  # instance_type = var.ec2_instance_type
  instance_type = var.instance_type
  ami           = var.ec2_ami_id #ubuntu

 

  root_block_device {
    # volume_size = var.ec2_root_storage_size
    volume_size = var.env == "prod" ? 20 : 10
    volume_type = "gp3"
  }
  # tags = {
  #   Name = "terraform-automate"
  # }
  tags = {
    Name = "${var.env}-infra-app-instance"
  }

}

```
outside the above file 
```6.tf
module "name" {
  source = "./infra-app"
  env = "dev"
  bucket_name = "tws-infra-app-bucket1"
  instance_count = 1
  instance_type = "ap-south-1"
  ec2_ami_id = "ami-02b8269d5e85954ef" #Amazon linux
  hash_key = "LockID"
}
```
also provide.tf and terraform.tf

