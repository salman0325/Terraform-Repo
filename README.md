## 1. Terraform Installation

### 1.1 Update package list
```bash
sudo apt update
````

**Explanation:** Package list ko update karta hai taake latest versions mil saken.

### 1.2 Terraform install karne ke liye dependencies install karo

```bash
sudo apt install -y gnupg software-properties-common curl
```

**Explanation:** Kuch zaroori tools aur libraries install karta hai jo Terraform ke liye chahiye hoti hain.

### 1.3 HashiCorp ka GPG key add karo

```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
```

**Explanation:** HashiCorp ki repository ke packages ko verify karne ke liye GPG key add karta hai.

### 1.4 HashiCorp repository add karo

```bash
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```

**Explanation:** Terraform packages ke liye official repo add karta hai.

### 1.5 Dubara package list update karo

```bash
sudo apt update
```

**Explanation:** Naya repo add karne ke baad packages list update karta hai.

### 1.6 Terraform install karo

```bash
sudo apt install terraform
```

**Explanation:** Terraform tool apne system par install karta hai.

### 1.7 Terraform version check karo

```bash
terraform version
```

**Explanation:** Confirm karta hai ke Terraform sahi se install hua hai.

---

## 2. Terraform Project Setup aur Commands

### 2.1 Current directory mein terraform initialize karo

```bash
terraform init
```

**Explanation:** Terraform modules aur providers download karta hai jo configuration mein hain.

### 2.2 Configuration files validate karo

```bash
terraform validate
```

**Explanation:** Syntax aur configurations mein errors check karta hai.

### 2.3 Terraform plan banao

```bash
terraform plan
```

**Explanation:** Batata hai ke infrastructure mein kya changes aayenge (dry run).

### 2.4 Infrastructure apply karo (create/update)

```bash
terraform apply
```

**Explanation:** Actual infrastructure create ya update karta hai. Ye aap se confirmation bhi maang sakta hai.

### 2.5 Infrastructure bina confirmation ke apply karo

```bash
terraform apply -auto-approve
```

**Explanation:** Confirmation ke bina changes apply karta hai.

### 2.6 Infrastructure destroy karo

```bash
terraform destroy
```

**Explanation:** Infrastructure ko delete kar deta hai.

### 2.7 Specific resource destroy karo

```bash
terraform destroy -target=module.name.aws_instance.example
```

**Explanation:** Sirf ek specific resource ko destroy karta hai.

### 2.8 Terraform state dekhne ke liye

```bash
terraform state list
```

**Explanation:** Current managed resources ki list dikhata hai.

### 2.9 Terraform output values dekho

```bash
terraform output
```

**Explanation:** Jo output variables declare kiye gaye hain unki values show karta hai.

### 2.10 Terraform fmt command

```bash
terraform fmt
```

**Explanation:** Configuration files ko standard format mein convert karta hai.

### 2.11 Terraform graph generate karo

```bash
terraform graph | dot -Tsvg > graph.svg
```

**Explanation:** Infrastructure dependency graph banata hai.

---

## 3. Module Specific Commands aur Variables

### 3.1 Module define karna

```hcl
module "name" {
  source = "./infra-app"
  env = "dev"
  bucket_name = "tws-infra-app-bucket1"
  instance_count = 1
  instance_type = "t2.micro"
  ec2_ami_id = "ami-02b8269d5e85954ef"
  hash_key = "LockID"
}
```

**Explanation:** Local module ko call karta hai jisme environment aur resources ke variables diye gaye hain.

### 3.2 Module ka plan check karo

```bash
terraform plan -target=module.name
```

**Explanation:** Sirf module ke changes ka plan dekhta hai.

### 3.3 Module apply karo

```bash
terraform apply -target=module.name
```

**Explanation:** Sirf specified module ke resources create ya update karta hai.

---

## 4. AWS CLI Basic Commands (Terraform ke sath use ke liye)

### 4.1 AWS configure karo (Access key aur region set karne ke liye)

```bash
aws configure
```

**Explanation:** AWS credentials aur region set karta hai.

### 4.2 S3 bucket list karo

```bash
aws s3 ls
```

**Explanation:** AWS account mein available S3 buckets show karta hai.

### 4.3 EC2 instance list karo

```bash
aws ec2 describe-instances
```

**Explanation:** Apke account ke EC2 instances ki details dikhata hai.

---

## 5. Helpful Linux Commands for Terraform

### 5.1 Directory ka structure dekho

```bash
ls -lah
```

**Explanation:** Current directory ki files aur folders detail mein dikhata hai.

### 5.2 File contents dekhne ke liye

```bash
cat filename.tf
```

**Explanation:** Terraform file ki content show karta hai.

### 5.3 File edit karne ke liye (nano editor)

```bash
nano filename.tf
```

**Explanation:** File ko edit karne ke liye nano editor open karta hai.

### 5.4 Current working directory check karo

```bash
pwd
```

**Explanation:** Aap kis folder mein ho, wo batata hai.

---

## 6. Terraform Workspaces Commands

### 6.1 Workspace list karo

```bash
terraform workspace list
```

**Explanation:** Available workspaces dikhata hai.

### 6.2 New workspace banao

```bash
terraform workspace new dev
```

**Explanation:** Naya workspace create karta hai.

### 6.3 Workspace switch karo

```bash
terraform workspace select dev
```

**Explanation:** Existing workspace par switch karta hai.

### 6.4 Current workspace dekho

```bash
terraform workspace show
```

**Explanation:** Jo workspace currently active hai wo dikhata hai.

---

## 7. Miscellaneous Commands

### 7.1 Terraform show current state

```bash
terraform show
```

**Explanation:** Current infrastructure ki detailed state show karta hai.

### 7.2 Terraform import existing resource

```bash
terraform import aws_instance.example i-1234567890abcdef0
```

**Explanation:** Existing AWS resource ko Terraform state mein add karta hai.

---

# Summary

* Pehle Terraform install karo (commands 1-7)
* Project initialize karo aur plan/apply karo (commands 8-18)
* Module ko define aur deploy karo (commands 19-21)
* AWS CLI aur Linux commands se support karo (commands 22-30)


Aapko ye README kaisa laga? Kya aur koi cheez add karun?
```
