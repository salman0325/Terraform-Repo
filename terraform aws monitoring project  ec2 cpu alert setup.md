```
# AWS provider define kar rahe hain
provider "aws" {
  region = "us-east-1"   # yahan apna region select karein
}

# EC2 instance create kar rahe hain
resource "aws_instance" "myec2" {   # resource ka naam myec2 rakha taake output mein match kare

  ami = "resolve:ssm:/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64"  
  # latest Amazon Linux 2023 AMI use kar rahe hain

  instance_type = "t3.micro"   # free tier instance type

  tags = {
    Name = "HelloWorld"   # instance ka naam tag
  }
}

# CloudWatch alarm create kar rahe hain
resource "aws_cloudwatch_metric_alarm" "myalarm" {

  alarm_name          = "terraform-test-foobar5"  
  # alarm ka naam

  comparison_operator = "GreaterThanOrEqualToThreshold"  
  # jab CPU threshold se zyada ho jaye

  evaluation_periods  = 1  
  # kitni dafa check kare

  metric_name         = "CPUUtilization"  
  # CPU metric monitor karega

  namespace           = "AWS/EC2"  
  # EC2 ka namespace

  period              = 300  
  # 5 minute ka period

  statistic           = "Average"  
  # average CPU calculate karega

  threshold           = 80  
  # 80% se zyada ho to alarm trigger

  alarm_description   = "This metric monitors ec2 cpu utilization"  
  # alarm ka description

  insufficient_data_actions = []  
  # insufficient data par koi action nahi

  dimensions = {
    InstanceId = aws_instance.myec2.id  
    # yahan hardcoded ID ki jagah EC2 resource ka ID use kiya
  }
}

# Output section

output "kke_instance_name" {
  value = aws_instance.myec2.tags["Name"]  
  # EC2 ka Name tag show karega
}

output "kke_alarm_name" {
  value = aws_cloudwatch_metric_alarm.myalarm.alarm_name  
  # alarm ka naam show karega
}

```
