
* User create hoga
* Group create hoga
* Policy create hoga
* User ko policy aur group ke sath attach karenge


---

# 📄 iam.tf

```hcl id="iam-full"
# ===============================
# IAM User, Group aur Policy Terraform File
# ===============================

# -------------------------------
# IAM User Create karna
# -------------------------------
resource "aws_iam_user" "dev_user" {
  name = "dev-user"           # IAM user ka naam
  path = "/"                  # Root path mein create kar rahe hain
  tags = {
    Environment = "Dev"       # Tag, optional, easily identify karne ke liye
  }
}

# -------------------------------
# IAM Group Create karna
# -------------------------------
resource "aws_iam_group" "dev_group" {
  name = "dev-group"          # Group ka naam
  path = "/"
  tags = {
    Environment = "Dev"
  }
}

# -------------------------------
# IAM Policy Create karna
# -------------------------------
resource "aws_iam_policy" "dev_policy" {
  name        = "dev-policy"          # Policy ka naam
  description = "Policy for Dev User" # Description
  path        = "/"

  policy = jsonencode({
    Version = "2012-10-17"           # AWS IAM policy version
    Statement = [
      {
        Effect = "Allow"              # Permission allow kar rahe hain
        Action = [
          "ec2:Describe*",            # EC2 ke describe actions allow
          "s3:ListBucket",            # S3 buckets list kar sakta hai
          "s3:GetObject"              # S3 objects read kar sakta hai
        ]
        Resource = "*"                 # Sab resources ke liye
      }
    ]
  })
}

# -------------------------------
# User ko Group mein add karna
# -------------------------------
resource "aws_iam_user_group_membership" "dev_user_group" {
  user = aws_iam_user.dev_user.name   # User ko specify karte hain
  groups = [aws_iam_group.dev_group.name]  # User ko ye group assign kar rahe hain
}

# -------------------------------
# Policy ko Group ke sath attach karna
# -------------------------------
resource "aws_iam_group_policy_attachment" "dev_group_attach" {
  group      = aws_iam_group.dev_group.name      # Group jiske sath attach karna hai
  policy_arn = aws_iam_policy.dev_policy.arn     # Policy ARN
}

# -------------------------------
# Optional: Directly User ko Policy attach karna
# -------------------------------
resource "aws_iam_user_policy_attachment" "dev_user_attach" {
  user       = aws_iam_user.dev_user.name        # User jisko policy deni hai
  policy_arn = aws_iam_policy.dev_policy.arn     # Policy ARN
}
```

---


1. **User** → `aws_iam_user` se create hota hai (`dev-user`)
2. **Group** → `aws_iam_group` se create hota hai (`dev-group`)
3. **Policy** → `aws_iam_policy` se create hoti hai (JSON actions define kiye)
4. **User ko Group mein dalna** → `aws_iam_user_group_membership`
5. **Policy ko Group ke sath attach** → `aws_iam_group_policy_attachment`
6. **Optional Direct User Attach** → `aws_iam_user_policy_attachment`

