

# **1️⃣ provider.tf** – AWS provider setup

```hcl
# AWS provider set karna
# Region apne requirement ke mutabiq change karo
provider "aws" {
  region = "us-east-1"
}
```

---

# **2️⃣ network.tf** – VPC, Subnets, Internet Gateway

```hcl
# 1. VPC create karna
resource "aws_vpc" "my_vpc" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "my-production-vpc"
  }
}

# 2. Public Subnet
resource "aws_subnet" "public_subnet" {
  vpc_id                  = aws_vpc.my_vpc.id
  cidr_block              = "10.0.1.0/24"
  map_public_ip_on_launch = true
  availability_zone       = "us-east-1a"
  tags = { Name = "public-subnet" }
}

# 3. Internet Gateway
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.my_vpc.id
  tags = { Name = "my-igw" }
}

# 4. Route Table
resource "aws_route_table" "public_rt" {
  vpc_id = aws_vpc.my_vpc.id
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }
  tags = { Name = "public-rt" }
}

# 5. Associate Route Table with Subnet
resource "aws_route_table_association" "public_assoc" {
  subnet_id      = aws_subnet.public_subnet.id
  route_table_id = aws_route_table.public_rt.id
}
```

---

# **3️⃣ security.tf** – Security Groups

```hcl
# Security Group for EC2
resource "aws_security_group" "web_sg" {
  name        = "web-sg"
  description = "Allow SSH and HTTP"
  vpc_id      = aws_vpc.my_vpc.id

  # Inbound rules
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] # SSH access
    description = "Allow SSH from anywhere"
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] # HTTP access
    description = "Allow HTTP from anywhere"
  }

  # Outbound rules
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    description = "Allow all outbound"
  }

  tags = { Name = "web-sg" }
}
```

---

# **4️⃣ ec2.tf** – EC2 Instance

```hcl
# EC2 instance create karna
resource "aws_instance" "web_server" {
  ami           = "ami-0c94855ba95c71c99" # apne region ke AMI ID
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.public_subnet.id
  security_groups = [aws_security_group.web_sg.name]

  tags = { Name = "web-server" }
}
```

---

# **5️⃣ s3.tf** – S3 Bucket

```hcl
# Static files / backups ke liye S3 bucket
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-production-bucket-12345" # unique name required
  acl    = "private"

  tags = { Name = "production-bucket" }
}
```

---

# **6️⃣ rds.tf** – RDS Database

```hcl
# RDS MySQL instance
resource "aws_db_subnet_group" "db_subnet" {
  name       = "my-db-subnet-group"
  subnet_ids = [aws_subnet.public_subnet.id]
  tags = { Name = "db-subnet-group" }
}

resource "aws_db_instance" "my_db" {
  allocated_storage    = 20
  engine               = "mysql"
  engine_version       = "8.0"
  instance_class       = "db.t2.micro"
  name                 = "mydatabase"
  username             = "admin"
  password             = "Password123!" # secure rakho
  db_subnet_group_name = aws_db_subnet_group.db_subnet.name
  vpc_security_group_ids = [aws_security_group.web_sg.id]
  skip_final_snapshot  = true

  tags = { Name = "production-db" }
}
```

---

# **7️⃣ cloudfront.tf** – CloudFront Distribution

```hcl
# S3 bucket ke liye CloudFront CDN
resource "aws_cloudfront_distribution" "my_cdn" {
  origin {
    domain_name = aws_s3_bucket.my_bucket.bucket_regional_domain_name
    origin_id   = "S3-my-bucket"
  }

  enabled             = true
  is_ipv6_enabled     = true
  default_root_object = "index.html"

  default_cache_behavior {
    allowed_methods  = ["GET", "HEAD"]
    cached_methods   = ["GET", "HEAD"]
    target_origin_id = "S3-my-bucket"

    forwarded_values {
      query_string = false
      cookies {
        forward = "none"
      }
    }

    viewer_protocol_policy = "redirect-to-https"
  }

  price_class = "PriceClass_100"

  restrictions {
    geo_restriction {
      restriction_type = "none"
    }
  }

  viewer_certificate {
    cloudfront_default_certificate = true
  }

  tags = { Name = "production-cdn" }
}
```

---

### ✅ **Usage Steps**

1. Initialize Terraform:

```bash
terraform init
```

2. Plan:

```bash
terraform plan
```

3. Apply:

```bash
terraform apply
```

