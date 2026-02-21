```tf

# ========================================
# AWS VPC, Subnet, Security Group aur EC2
# ========================================

# -------------------------------
# VPC create karna
# -------------------------------
resource "aws_vpc" "main" {
  cidr_block           = var.KKE_VPC_CIDR        # Variable se CIDR
  enable_dns_support   = true                     # DNS support enable
  enable_dns_hostnames = true                     # Hostnames enable

  tags = {
    Name = "main"                                 # VPC ka naam
  }
}

# -------------------------------
# Subnet create karna
# -------------------------------
resource "aws_subnet" "main" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = var.KKE_SUBNET_CIDR   # Variable se CIDR
  map_public_ip_on_launch = true                  # Public IP EC2 ko mile launch par

  tags = {
    Name = "Main"                                 # Subnet ka naam
  }
}

# -------------------------------
# Security Group
# -------------------------------
resource "aws_security_group" "my_security_group" {
  name        = "allow_tls"
  description = "Allow all traffic within VPC"
  vpc_id      = aws_vpc.main.id

  # Outbound traffic (egress)
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"            # Sab protocols allow
    cidr_blocks = ["0.0.0.0/0"]  # Sab IPv4 traffic allow
    ipv6_cidr_blocks = ["::/0"]   # Sab IPv6 traffic allow
  }

  # Inbound traffic (ingress)
  ingress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"            # Sab protocols allow
    cidr_blocks = ["0.0.0.0/0"]  # Sab IPv4 allow
    ipv6_cidr_blocks = ["::/0"]   # Sab IPv6 allow
  }

  tags = {
    Name = "my-security-group"
  }
}

# -------------------------------
# EC2 Instance
# -------------------------------
resource "aws_instance" "example" {
  ami                    = "ami-0c02fb55956c7d316" # Example Amazon Linux 2 AMI, region ke hisab se change karo
  instance_type          = "t3.micro"
  subnet_id              = aws_subnet.main.id
  vpc_security_group_ids = [aws_security_group.my_security_group.id]

  tags = {
    Name = "HelloWorld"                           # EC2 ka naam
  }
}

# -------------------------------
# Variables
# -------------------------------
variable "KKE_VPC_CIDR" {
  description = "CIDR block for VPC"
  default     = "10.0.0.0/16"
}

variable "KKE_SUBNET_CIDR" {
  description = "CIDR block for Subnet"
  default     = "10.0.1.0/24"
}

# -------------------------------
# Outputs
# -------------------------------
output "kke_vpc_name" {
  value = aws_vpc.main.tags["Name"]             # VPC ka naam output
}

output "kke_subnet_name" {
  value = aws_subnet.main.tags["Name"]          # Subnet ka naam output
}

output "kke_ec2_name" {
  value = aws_instance.example.tags["Name"]     # EC2 ka naam output
}
```
