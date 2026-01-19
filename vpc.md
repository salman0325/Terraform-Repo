terraform modules
what is modules?
modules are reusible code blok of code you use a source and using the source you can carete what ever you want.


go to the terraform registroy

search for the vpc and select terraform-aws-modules pay click kardonga 


how to create custum modules?
create your own mudues

```terraform.tf
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "6.22.1"
    }
  }
}



provider "aws" {
  region = "ap-south-1"

}
```
```terraform.tf
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"

  name = "tws-vpc-my-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["ap-south-1a", "ap-south-1b"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24"]

  enable_nat_gateway = true
  enable_vpn_gateway = true

  tags = {
    Terraform = "true"
    Environment = "dev"
  }
}
```

