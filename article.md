---
author: "Kyle Jones"
date_published: "April 19, 2025"
date_exported_from_medium: "November 10, 2025"
canonical_link: "https://medium.com/@kyle-t-jones/terraform-without-the-fluff-how-hcl-works-and-what-your-project-files-should-look-like-7e400c3813d2"
---

# Terraform, Without the Fluff: How HCL Works and What Your Project Files Should Look Like Most people are surprised the first time they read a Terraform file. It
looks like JSON --- but cleaner. That's because Terraform uses HCL2...

### Terraform, Without the Fluff: How HCL Works and What Your Project Files Should Look Like
Most people are surprised the first time they read a Terraform file. It looks like JSON --- but cleaner. That's because Terraform uses HCL2, the HashiCorp Configuration Language. It's a purpose-built syntax designed for human readability without giving up structure.

Let's start with something familiar. Here's what it looks like to launch a simple EC2 instance:

``` 
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

No semicolons. No deep nesting. Just a resource block, a name, and some attributes.

But understanding HCL is only half the story. The other half is knowing how to structure your Terraform project so you don't lose your mind as it grows.

### The Anatomy of a Terraform Project
Here's the layout that most teams adopt once they move beyond one-off scripts:

``` 
project-root/
├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── backend.tf
└── modules/
```

Each of these files plays a distinct role. Keep them separate, and you keep your sanity.

- **`main.tf`** is your blueprint. This is where your actual infrastructure lives. Want to spin up an EC2 instance, configure a VPC, or define a security group? It all happens here.
- **`variables.tf`** declares your inputs. Instead of hardcoding values, you define what should be configurable---region, instance type, environment, you name it.
- **`outputs.tf`** makes values available after Terraform runs. That might be the public IP of an instance, an ARN, or a database connection string you need downstream.
- **`terraform.tfvars`** is where you set the actual values for your variables. It keeps your config clean and reusable.
- **`backend.tf`** defines how and where Terraform stores state---whether that's a local file or an S3 bucket for team use.
- **`modules/`** is where you start thinking like a software engineer. Modularize your code. Keep reusable pieces (like networking, compute, or IAM roles) in their own folders so you can plug them into multiple projects without rewriting anything.

### Why This Structure Matters
Terraform's flexibility is a double-edged sword. It lets you do almost anything, but that freedom can quickly turn into chaos if you don't impose some discipline. This file structure enforces a contract. It helps you separate logic, inputs, outputs, and state so that the project remains readable, testable, and maintainable.

That matters when you're working alone. It matters even more when you're collaborating across teams or managing infrastructure at scale.

### Terraform Workflow
Terraform doesn't just help you spin up infrastructure --- it gives you a system. A rhythm. A repeatable workflow that keeps your cloud resources under control, even as your team, requirements, and environments grow more complex.

Let's walk through the Terraform workflow the way it's meant to be used: not just in a hello-world demo, but in a living, breathing infrastructure project.

### Write: Declare What You Want
Every Terraform project starts with a set of `.tf` files written in HCL---the HashiCorp Configuration Language. This is where you describe the infrastructure you want to create. Not how to create it. Just *what* it should look like.

You define providers like AWS, Azure, or Google Cloud. You declare resources like `aws_instance`, `azurerm_storage_account`, or `google_container_cluster`. You group logic with modules and pass in variables for flexibility.

Here's a simple example:

``` 
provider "aws" {
  region = "us-west-2"
}
```

``` 
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

Think of it like writing a blueprint. Terraform doesn't care about the order. It figures out dependencies for you.

### Init: Bootstrap Your Project
Next comes `terraform init`. This command sets up your working directory. It installs the necessary providers, initializes backend configuration, and gets your environment ready to run.

You only need to do this once --- unless you change providers or backend settings. It's like preparing your workspace before you build anything.

``` 
terraform init
```

Behind the scenes, Terraform pulls the provider plugins (like AWS, Azure, etc.) and configures the backend if you're using remote state.

### Plan: See What Terraform Will Do
With your configuration ready, the next step is to preview the changes. That's where `terraform plan` comes in.

``` 
terraform plan
```

This is Terraform's dry run. It tells you what *would* happen if you applied your configuration now. What resources would be created, modified, or destroyed? Are there any errors in your code? Any mismatches between current and desired state?

The plan phase is your safety net. In teams, this is often reviewed in pull requests before anything gets applied.

### Apply: Make the Change
Now the moment of truth: `terraform apply`.

``` 
terraform apply
```

Terraform asks for confirmation before it makes any changes. Once you approve, it builds the infrastructure you defined. If you're using version control and remote state, this step is predictable, auditable, and safe to run in automated pipelines.

### Destroy: Clean Up When You're Done
When you no longer need a resource --- or an entire environment --- Terraform gives you a clean way to tear it all down.

``` 
terraform destroy
```

No more dangling EC2 instances or zombie containers eating up your budget. No more forgotten test environments. `terraform destroy` makes it easy to clean up with confidence.

### Where It All Comes Together
You can run this workflow on your local machine or in a CI/CD pipeline. And once you start layering in remote state (like S3 + DynamoDB), testing tools (like `terratest`), and automation, Terraform becomes more than a tool. It becomes a system of control.

That's why experienced teams treat this workflow not as a suggestion, but as muscle memory.

Write.\ Init.\ Plan.\ Apply.\ Destroy.

Repeat.

### Next Steps
If you're new to Terraform, try running this workflow on a small project. If you're already using it, consider how you can build more automation and safety into each step. Infrastructure as code only delivers its value when you use it deliberately.

Want more real-world Terraform use cases? Stay tuned --- I've got examples coming up from CI/CD pipelines, module design, and state management in complex environments.
