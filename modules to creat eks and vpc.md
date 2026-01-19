provide.tf
terraform.tf
create first 

create local.tf file
```te.tf
locals {
  region = "ap-south-1"
  name = "tws-eks-cluster"
  vpc_cidr = "10.0.0.0/16"
  env = "dev"
}
```
to create a vpc using modules
```terraform.tf
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"

  name = "${local.name}-vpc"
  cidr = local.vpc_cidr

  azs             =  ["ap-south-1a", "ap-south-1b"]
  private_subnets = ["10.0.0.0/24", "10.0.2.0/24"]
  public_subnets  =  ["10.0.101.0/24", "10.0.102.0/24"]
  intra_subnets = ["10.0.5.0/24", "10.0.6.0/24"]
 

  enable_nat_gateway = true
  enable_vpn_gateway = true


  tags = {
    Terraform = "true"
    Environment = local.env
  }
}
```
create a eks cluster suing modules
```terraform.tf
module "eks" {
    source  = "terraform-aws-modules/eks/aws"
    version = "20.31.0"
    cluster_name = local.name
    cluster_version = "1.31"
    cluster_endpoint_public_access = true

    vpc_id = module.vpc.vpc_id
    subnet_ids = module.vpc.private_subnets
    control_plane_subnet_ids = module.vpc.intra_subnets

    tags = {
        Environment = local.env
        Terraform = "true"
    }
    
  
}
```
