## Terraform Important Interview Questions & Answers (Roman Urdu)

### 1. Terraform kya hai?

**Answer:**
Terraform ek **Infrastructure as Code (IaC)** tool hai jo HashiCorp ne banaya hai. Is ki madad se hum AWS, Azure aur GCP par infrastructure ko code likh kar create, update aur delete kar sakte hain.

---

### 2. Infrastructure as Code (IaC) kya hota hai?

**Answer:**
Infrastructure ko manually banane ke bajaye code ke zariye create aur manage karna Infrastructure as Code kehlata hai.

Example:

* EC2
* VPC
* S3 Bucket
* Database

---

### 3. Terraform ke kya advantages hain?

**Answer:**

* Automation
* Repeatable Infrastructure
* Version Control
* Kam human errors
* Multi-cloud support (AWS, Azure, GCP)

---

### 4. Provider kya hota hai?

**Answer:**
Provider Terraform ko kisi cloud ya service se connect karta hai.

Example:

* AWS Provider
* Azure Provider
* Google Provider
* Kubernetes Provider

Agar AWS provider use kar rahe hain to Terraform AWS ke resources create karega.

---

### 5. Resource kya hota hai?

**Answer:**
Resource woh cheez hai jo Terraform create ya manage karta hai.

Example:

* EC2 Instance
* VPC
* S3 Bucket
* Security Group

Har resource `resource` block ke andar likha jata hai.

---

### 6. Data Source kya hota hai?

**Answer:**
Data Source existing resources ki information lene ke liye use hota hai.

Yeh naya resource create nahi karta, sirf information read karta hai.

---

### 7. Variable kya hota hai?

**Answer:**
Variable ek aisi value hoti hai jo change ki ja sakti hai.

Example:
Aaj region `us-east-1` hai, kal `ap-south-1` karna ho to sirf variable change karna padega.

---

### 8. Output kya hota hai?

**Answer:**
Output kisi resource ki information screen par dikhata hai.

Example:

* Public IP
* Instance ID
* DNS Name

---

### 9. terraform init kya karta hai?

**Answer:**
Ye project ko initialize karta hai.

Is command se:

* Providers download hote hain.
* Backend initialize hota hai.
* Terraform project ready hota hai.

Ye hamesha sab se pehla command hota hai.

---

### 10. terraform plan kya karta hai?

**Answer:**
Ye batata hai ke Terraform kya changes karega.

Example:

* EC2 create hoga.
* VPC create hogi.
* S3 delete hogi.

Lekin is stage par kuch create nahi hota.

---

### 11. terraform apply kya karta hai?

**Answer:**
Ye actual infrastructure create ya update karta hai.

Is command ke baad AWS par resources ban jate hain.

---

### 12. terraform destroy kya karta hai?

**Answer:**
Terraform ke banaye hue tamam resources delete kar deta hai.

---

### 13. Terraform State kya hai?

**Answer:**
Terraform State (`terraform.tfstate`) ek file hoti hai jisme Terraform record rakhta hai ke kaun se resources create ho chuke hain.

---

### 14. State File kyun zaroori hai?

**Answer:**
State file ke bina Terraform ko pata nahi chalega ke pehle kya create hua tha.

Isi file ki madad se Terraform update aur delete karta hai.

---

### 15. Backend kya hota hai?

**Answer:**
Backend woh jagah hai jahan Terraform State file store hoti hai.

Example:

* Local Machine
* AWS S3
* Azure Storage
* Terraform Cloud

---

### 16. Remote Backend kyun use karte hain?

**Answer:**

* Team ke sath state share karne ke liye.
* Backup ke liye.
* State Locking ke liye.
* Collaboration ke liye.

---

### 17. Module kya hota hai?

**Answer:**
Module reusable Terraform code hota hai.

Ek hi module ko multiple projects mein use kiya ja sakta hai.

---

### 18. AMI kya hoti hai?

**Answer:**
AMI (Amazon Machine Image) ek template hoti hai jis se EC2 Instance create hota hai.

Example:

* Ubuntu
* Amazon Linux
* Red Hat

---

### 19. Security Group kya hota hai?

**Answer:**
Security Group EC2 ka firewall hota hai.

Ye decide karta hai ke kaun se ports open honge.

Example:

* Port 22 → SSH
* Port 80 → HTTP
* Port 443 → HTTPS

---

### 20. Key Pair kya hota hai?

**Answer:**
Key Pair EC2 mein secure SSH login ke liye use hoti hai.

* Public Key → AWS mein upload hoti hai.
* Private Key → Aap ke computer par rehti hai.

---

### 21. terraform validate kya karta hai?

**Answer:**
Terraform code ki syntax check karta hai.

Agar koi error ho to bata deta hai.

---

### 22. terraform fmt kya karta hai?

**Answer:**
Terraform code ko automatically proper format mein le aata hai.

---

### 23. terraform import kya karta hai?

**Answer:**
Existing AWS resource ko Terraform ke under laata hai taa ke Terraform usay manage kar sake.

---

### 24. Terraform aur CloudFormation mein kya difference hai?

**Answer:**

**Terraform**

* Multi-cloud support.
* AWS, Azure, GCP sab ko support karta hai.
* HashiCorp ka tool hai.

**CloudFormation**

* Sirf AWS ke liye hai.
* Amazon ka apna tool hai.

---

### 25. Terraform ki common files kaunsi hoti hain?

* `main.tf` → Resources
* `providers.tf` → Provider configuration
* `variables.tf` → Variables
* `outputs.tf` → Outputs
* `terraform.tfvars` → Variable values
* `terraform.tfstate` → State file

---

## Sab se Important Interview Question

### Terraform Workflow kya hota hai?

**Answer:**

```text
terraform init
      ↓
terraform validate
      ↓
terraform plan
      ↓
terraform apply
      ↓
Infrastructure Create
```

