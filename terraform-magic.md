---
pubDate: 2024-06-13
updatedDate: 2024-06-13
title: Terraform magic
description: I'm learning about Terraform and it's all magic to me
featured: false
draft: false
topics:
  - infrastructure
  - devops
---
I'm learning about [Terraform](https://developer.hashicorp.com/terraform), and it's all magic to me right now. I know that Terraform is an Infrastructure as Code (IaC) tool. The alternative to IaC is clicking around GUIs to create your infrastructure. I'm not going to lie, the AWS UI is overwhelming. Also, it's basically all acronyms, and [I hate acronyms](https://www.inc.com/jeff-steen/why-you-should-stop-using-acronyms-right-now.html). 

I fully support Infrastructure as Code (IaC). It's safe, consistent, and repeatable. You can version your infrastructure, share it, and reuse it. Now back to Terraform. How does it work?

Here's my best guess with the little research I've done so far.

Terraform has a provider registry that you can reference in your `tf` file. A provider is a plugin for a specific infrastructure service, a service like AWS or Digital Ocean, for example. When you run `terraform apply`, the tool will read your configuration and call some APIs in the provider, which in turn hook into APIs on the infrastructure service that will create / modify / delete the infrastructure. Your Terraform configuration goes into `main.tf`, which is a naming convention, or it can go into any other `*.tf` file.

The next step is to use Terraform to set up some infrastructure for a Rails application. I'm planning on going through the [Core Terraform Workflow](https://developer.hashicorp.com/terraform/intro/v1.1.x/core-workflow) for guidance.