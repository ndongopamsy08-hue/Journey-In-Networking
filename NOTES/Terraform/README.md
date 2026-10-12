# Terraform – CCNA Study Notes

Summary notes on Terraform and Infrastructure as Code (IaC) for the CCNA network automation topics.

## Contents

1. [Infrastructure as Code (IaC)](#1-infrastructure-as-code-iac)
2. [Provisioning vs Configuration Management](#2-provisioning-vs-configuration-management)
3. [Mutable vs Immutable Infrastructure](#3-mutable-vs-immutable-infrastructure)
4. [What is Terraform?](#4-what-is-terraform)
5. [Terraform Workflow](#5-terraform-workflow)
6. [HCL and DSLs](#6-hcl-and-dsls)
7. [Quick Review](#7-quick-review)

---

## 1. Infrastructure as Code (IaC)

- **IaC** is the practice of provisioning and managing infrastructure (servers, networks, cloud resources) using **machine-readable code files** instead of manual configuration (e.g. CLI or GUI).
- IaC automates infrastructure deployment and management, ensuring **consistency, scalability and repeatability**.
- **Terraform** is an IaC-based **provisioning tool** that automates the creation of infrastructure resources.

## 2. Provisioning vs Configuration Management

| | Configuration Management | Infrastructure Provisioning |
|---|---|---|
| **Examples** | Ansible, Puppet, Chef | Terraform |
| **Purpose** | Manages **existing** infrastructure | Creates, modifies and deletes infrastructure resources (servers, networks) |
| **Focus** | Installing software, configuring settings, maintaining system state | Initial setup rather than ongoing configuration management |
| **Result** | Consistency by applying and enforcing configurations across many devices | Infrastructure built from scratch |

- Configuration management tools work on **already existing** systems, whereas provisioning tools **build infrastructure from scratch**.
- **Terraform and Ansible can work together:**
  - Terraform provisions the infrastructure (VMs, networks, storage).
  - Ansible provides ongoing configuration and management.

## 3. Mutable vs Immutable Infrastructure

| | Mutable | Immutable |
|---|---|---|
| **Used by** | Configuration management tools | Provisioning tools |
| **Changes after deployment** | Allowed (updates, patches, configuration changes) | Not allowed |
| **How changes are made** | **In place** – existing resources are updated | Old resources are **replaced** with a new version |
| **Configuration drift** | Possible | None – each deployment starts from a predefined state |

## 4. What is Terraform?

- **Open source** IaC tool developed by **HashiCorp** (acquired by IBM in 2025).
- Primarily a **provisioning** tool, focused on deploying infrastructure resources on various **cloud and on-prem** platforms.
- These platforms are called **providers**, and include AWS, Azure, GCP, Kubernetes and many more – including integrations with Cisco platforms such as **Catalyst Center**.
- Like Ansible, Terraform uses a **push model** and is **agentless** – it does not require software agents on the infrastructure it provisions or manages.

## 5. Terraform Workflow

The basic workflow has three main steps:

| Step | Description |
|---|---|
| **Write** | Define the **desired state** of your infrastructure resources in configuration files |
| **Plan** | Verify the changes that will be made **before** applying them |
| **Apply** | Execute the plan to provision and manage the infrastructure resources |

## 6. HCL and DSLs

- Terraform core is written in **Go**.
- Configuration files are written in **HashiCorp Configuration Language (HCL)**.
- HCL is an example of a **Domain-Specific Language (DSL)** – a language specialised for a particular purpose.
- Because DSLs are specialised, they let users perform complex tasks with **much less code** than a general-purpose language such as Python or Go would require.

## 7. Quick Review

- IaC = infrastructure defined in code, not manual config.
- Terraform = **provisioning**; Ansible/Puppet/Chef = **configuration management**.
- Provisioning tools → **immutable**; configuration management tools → **mutable**.
- Terraform is **agentless**, uses a **push model**, and talks to platforms through **providers**.
- Workflow: **Write → Plan → Apply**.
- Config language: **HCL** (a DSL); Terraform core is written in **Go**.
