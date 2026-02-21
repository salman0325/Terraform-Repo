

* VPC
* Public + Private Subnet
* Internet Gateway
* Route Table
* Security Group
* EC2
* S3
* RDS
* IAM Role



---

# 📁 Project Structure

```
terraform-project/
│── provider.tf
│── variables.tf
│── vpc.tf
│── ec2.tf
│── s3.tf
│── rds.tf
│── iam.tf
│── outputs.tf
```

---

# 1️⃣ provider.tf

```hcl
# Yeh terraform ka required providers block hai
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"   # AWS provider use kar rahe hain
      version = "~> 5.0"          # Version 5 ke around koi bhi compatible version
    }
  }
}

# Yahan AWS provider configure karte hain
provider "aws" {
  region = var.region   # Region variable se aa raha hai (variables.tf mein define hoga)
}
```

---

# 2️⃣ variables.tf

```hcl
# AWS region define karne ke liye variable
variable "region" {
  description = "AWS region jahan resources create honge"
  default     = "us-east-1"
}

# VPC CIDR block
variable "vpc_cidr" {
  default = "10.0.0.0/16"
}

# Public subnet CIDR
variable "public_subnet_cidr" {
  default = "10.0.1.0/24"
}

# Private subnet CIDR
variable "private_subnet_cidr" {
  default = "10.0.2.0/24"
}
```

---

# 3️⃣ vpc.tf

```hcl
# VPC create kar rahe hain
resource "aws_vpc" "main_vpc" {
  cidr_block = var.vpc_cidr   # CIDR variable se aa raha hai

  tags = {
    Name = "main-vpc"   # VPC ka naam
  }
}

# Internet Gateway bana rahe hain
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main_vpc.id   # Is IGW ko VPC ke sath attach kar rahe hain

  tags = {
    Name = "main-igw"
  }
}

# Public subnet bana rahe hain
resource "aws_subnet" "public_subnet" {
  vpc_id                  = aws_vpc.main_vpc.id
  cidr_block              = var.public_subnet_cidr
  map_public_ip_on_launch = true   # EC2 ko public IP mile

  tags = {
    Name = "public-subnet"
  }
}

# Private subnet bana rahe hain
resource "aws_subnet" "private_subnet" {
  vpc_id     = aws_vpc.main_vpc.id
  cidr_block = var.private_subnet_cidr

  tags = {
    Name = "private-subnet"
  }
}

# Route table create kar rahe hain
resource "aws_route_table" "public_rt" {
  vpc_id = aws_vpc.main_vpc.id

  route {
    cidr_block = "0.0.0.0/0"   # Internet ke liye route
    gateway_id = aws_internet_gateway.igw.id
  }

  tags = {
    Name = "public-route-table"
  }
}

# Route table ko public subnet se attach kar rahe hain
resource "aws_route_table_association" "public_assoc" {
  subnet_id      = aws_subnet.public_subnet.id
  route_table_id = aws_route_table.public_rt.id
}
```

---

# 4️⃣ ec2.tf

```hcl
# Security group bana rahe hain
resource "aws_security_group" "web_sg" {
  name   = "web-sg"
  vpc_id = aws_vpc.main_vpc.id

  ingress {
    from_port   = 22      # SSH port
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]   # Sab jagah se allow (production mein restrict karna chahiye)
  }

  ingress {
    from_port   = 80   # HTTP port
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"   # Sab protocol allow
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# EC2 instance create kar rahe hain
resource "aws_instance" "web_server" {
  ami           = "ami-0c02fb55956c7d316"   # Example Amazon Linux AMI (region ke hisab se change karna)
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.public_subnet.id
  security_groups = [aws_security_group.web_sg.id]

  tags = {
    Name = "web-server"
  }
}
```

---

# 5️⃣ s3.tf

```hcl
# S3 bucket bana rahe hain
resource "aws_s3_bucket" "app_bucket" {
  bucket = "my-unique-terraform-bucket-12345"   # Globally unique naam hona chahiye

  tags = {
    Name = "app-bucket"
  }
}
```

---

# 6️⃣ rds.tf

```hcl
# RDS subnet group create kar rahe hain
resource "aws_db_subnet_group" "db_subnet_group" {
  name       = "db-subnet-group"
  subnet_ids = [aws_subnet.private_subnet.id]

  tags = {
    Name = "db-subnet-group"
  }
}

# RDS instance create kar rahe hain
resource "aws_db_instance" "mysql_db" {
  allocated_storage    = 20
  engine               = "mysql"
  instance_class       = "db.t3.micro"
  username             = "admin"
  password             = "Admin12345"   # Production mein variable ya secrets use karo
  db_subnet_group_name = aws_db_subnet_group.db_subnet_group.name
  skip_final_snapshot  = true
}
```

---

# 7️⃣ iam.tf

```hcl
# IAM role bana rahe hain
resource "aws_iam_role" "ec2_role" {
  name = "ec2-role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "ec2.amazonaws.com"
      }
    }]
  })
}
```

---

# 8️⃣ outputs.tf

```hcl
# EC2 ka public IP output kar rahe hain
output "ec2_public_ip" {
  value = aws_instance.web_server.public_ip
}

# S3 bucket name output
output "s3_bucket_name" {
  value = aws_s3_bucket.app_bucket.bucket
}
```

