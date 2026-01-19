-----------------command for tf.state file-----------------------
1) terraform state list
2) terraform state show aws_key_pair.my_key
3) terraform state rm aws_key_pair.my_key
4) terraform import aws_key_pair.my_key  key-0df156aa2f3db2296 
5) terrafrom refresh
6) terraform workspace list 
7) terraform workspace new dev
8) terraform workspace select default
9) terraform workspace select dev
10) terraform workspace select default
11) 
```terraform.tf
provider "aws" {
  region = "ap-south-1"

}
```
```terraform.tf
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "6.22.1"
    }
  }

  backend "s3" {
    bucket = "tws-junoon-state-bucketss"
    key = "terraform.tfstate"
    region = "ap-south-1"
    dynamodb_table = "tws-junoon-state-table"
  }
}
```
```terraform.tf
resource "aws_s3_bucket" "romote_s3" {
    bucket = "tws-junoon-state-bucketss"
    tags = {
      Name = "tws-junoon-state-bucket"
      
    }
  
}
```
```terraform.tf
resource "aws_dynamodb_table" "basic_dynamodb_table" {
  name         = "tws-junoon-state-table"
  billing_mode = "PAY_PER_REQUEST"

  hash_key     = "LockID"

  attribute {
    name = "LockID"
    type = "S"     # S = String, N = Number, B = Binary
  }

  tags = {
    Name = "tws-junoon-state-table"
  }
}
```




