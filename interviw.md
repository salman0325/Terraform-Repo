

# Terraform Interview Questions and Answers

---

## 1. What is Terraform?

**Answer:**
Terraform is an open-source Infrastructure as Code (IaC) tool created by HashiCorp. It allows you to define and provision infrastructure resources using a declarative configuration language called HCL (HashiCorp Configuration Language).

---

## 2. What are the main features of Terraform?

**Answer:**

* Infrastructure as Code
* Execution plans (preview changes before applying)
* Resource Graph (dependency management)
* Change Automation
* Multi-cloud support
* State management

---

## 3. What is a Terraform provider?

**Answer:**
A provider is a plugin responsible for managing the lifecycle of resources in a specific cloud or service. For example, AWS, Azure, Google Cloud providers.

---

## 4. What is Terraform state?

**Answer:**
Terraform state is a file that stores information about the infrastructure resources managed by Terraform. It keeps track of resource metadata and helps Terraform map the configuration to real-world resources.

---

## 5. Why is Terraform state important?

**Answer:**

* It tracks resource IDs and metadata
* Enables Terraform to perform incremental changes
* Helps with dependency management
* Allows collaboration when using remote state

---

## 6. What is the difference between `terraform apply` and `terraform plan`?

**Answer:**

* `terraform plan` shows what changes will be made without applying them.
* `terraform apply` actually applies those changes to the infrastructure.

---

## 7. What are Terraform modules?

**Answer:**
Modules are reusable configurations that encapsulate groups of resources. They help to organize and reuse code for infrastructure.

---

## 8. How do you create a Terraform module?

**Answer:**
Create a folder with `.tf` files containing resources and variables. Call the module using the `module` block with a `source` attribute pointing to the module path or registry.

---

## 9. What is Remote Backend in Terraform?

**Answer:**
A backend defines where Terraform's state is stored. Remote backend allows storing the state file in a remote service like AWS S3, Terraform Cloud, or Azure Blob Storage, enabling collaboration and locking.

---

## 10. How do you manage sensitive data in Terraform?

**Answer:**
Use environment variables, encrypted storage (e.g., Vault), or Terraform's sensitive variable attribute (`sensitive = true`). Avoid hardcoding secrets directly in configuration files.

---

## 11. What is the difference between `count` and `for_each`?

**Answer:**

* `count` creates multiple instances of a resource using an index.
* `for_each` creates instances based on keys of a map or elements of a set, providing more control and naming.

---

## 12. What happens if you delete the state file?

**Answer:**
Terraform loses track of the existing resources, which might cause it to recreate resources on the next apply, leading to potential conflicts or duplicates.

---

## 13. How does Terraform handle dependencies between resources?

**Answer:**
Terraform automatically infers dependencies by analyzing resource references. You can also explicitly define dependencies using the `depends_on` attribute.

---

## 14. Can you explain the Terraform lifecycle block?

**Answer:**
The lifecycle block controls resource behavior during creation, update, and deletion, e.g., `prevent_destroy` to avoid accidental resource deletion, or `create_before_destroy` to handle replacements.

---

## 15. How do you upgrade Terraform versions in your project?

**Answer:**

* Update the Terraform binary.
* Review the upgrade guides for any breaking changes.
* Run `terraform init -upgrade` to update provider plugins.
* Run `terraform plan` to check for issues.

---

## 16. What is the difference between `terraform import` and `terraform apply`?

**Answer:**

* `terraform import` imports existing infrastructure into Terraform state without modifying it.
* `terraform apply` creates or updates infrastructure based on config files.

---

## 17. Scenario: You want to create multiple EC2 instances with different sizes using Terraform. How would you do that?

**Answer:**
Use `for_each` or `count` with a map or list variable defining instance types. Example:

```hcl
variable "instances" {
  default = {
    instance1 = "t2.micro"
    instance2 = "t2.small"
  }
}

resource "aws_instance" "example" {
  for_each = var.instances

  ami           = "ami-123456"
  instance_type = each.value
}
```

---

## 18. Scenario: How do you handle Terraform state locking to prevent concurrent runs?

**Answer:**
Use a backend that supports state locking like S3 with DynamoDB (AWS), Terraform Cloud, or Azure Blob Storage. Locking prevents multiple users from applying changes simultaneously.

---

## 19. What is the purpose of `terraform fmt`?

**Answer:**
`terraform fmt` formats Terraform configuration files to a canonical style for readability and consistency.

---

## 20. How can you pass variables to Terraform?

**Answer:**

* Via CLI flags (`-var "key=value"`)
* Using `.tfvars` files
* Environment variables (`TF_VAR_key=value`)
* Default values in variable declarations

---

## 21. What is the `terraform refresh` command used for?

**Answer:**
It updates the Terraform state file with the real infrastructure state, syncing any changes made outside Terraform.

---

## 22. What are workspaces in Terraform?

**Answer:**
Workspaces allow managing multiple instances of infrastructure from a single configuration, isolating their state files.

---

## 23. Scenario: You need to update a resource without destroying it. How do you ensure this?

**Answer:**
Use the lifecycle block with `prevent_destroy = true` and design your configuration to avoid changes that force replacement (e.g., changing immutable fields).

---

## 24. How do you handle secrets securely with Terraform in CI/CD pipelines?

**Answer:**
Use secret management tools like HashiCorp Vault, AWS Secrets Manager, or environment variables injected securely by the pipeline. Never commit secrets in code or logs.

---

## 25. Scenario: Your Terraform apply fails due to a resource conflict. How do you troubleshoot?

**Answer:**

* Check the state file for drift or corruption.
* Run `terraform plan` to identify conflicts.
* Use `terraform refresh` to sync state.
* Use `terraform import` for unmanaged resources.
* Investigate provider-specific logs or cloud console.

---

# README.md File for Terraform Interview Questions

```markdown
# Terraform Interview Questions and Answers

This file contains 25 commonly asked Terraform interview questions and their answers, including scenario-based questions to help you prepare effectively.

---

## Questions and Answers

1. **What is Terraform?**  
Terraform is an open-source Infrastructure as Code (IaC) tool created by HashiCorp...

2. **What are the main features of Terraform?**  
- Infrastructure as Code  
- Execution plans  
- Resource Graph...

(Include all 25 Q&As here)

---

## Scenario-based Questions

- How to create multiple EC2 instances with different types using Terraform?  
- How do you handle Terraform state locking?  
- How to update a resource without destroying it?  
- Troubleshooting resource conflicts in Terraform apply.

---

## Useful Terraform Commands

- `terraform init` — Initialize a working directory  
- `terraform plan` — Preview changes  
- `terraform apply` — Apply changes  
- `terraform destroy` — Destroy resources  
- `terraform fmt` — Format code  
- `terraform import` — Import existing resources  
- `terraform refresh` — Sync state

---

## Tips

- Always use remote state for collaboration  
- Manage sensitive data securely  
- Modularize your code for reusability  
- Keep Terraform versions consistent

---

