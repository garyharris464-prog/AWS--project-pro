 Day 4: Amazon EC2 — Virtual Machines, Key Pairs & Security Groups

**Date:** [11/04/2026]  
**Status:** Completed  

---

## Overview & What I Learned
- **Virtualization & AMIs:** An Amazon Machine Image (AMI) provides the initial operating system, application server, and pre-installed packages needed to launch an EC2 instance.
- **Instance Types:** Understanding the naming scheme (e.g., `t2.micro` / `t3.micro`: `t` family is burstable general purpose, `2/3` generation, `micro` size).
- **Security Groups as Statefull Firewalls:** Rules evaluate inbound and outbound traffic; because they are stateful, return traffic is automatically allowed regardless of outbound rules.
- **Bootstrapping with User Data:** Using cloud-init scripts to automate software installation upon initial instance launch without manual SSH commands.

---

## What I Built / Configured
1. Provisioned a Free Tier Amazon Linux 2023 EC2 instance in `us-east-1.
2. Generated and securely saved an RSA `.pem` key pair for SSH access.
3. Configured an EC2 Security Group restricting inbound SSH (Port 22) to my current IP while allowing public HTTP (Port 80).
4. Bootstrapped Apache (`httpd`) via User Data and served a live web page over the public IPv4 address.

---

## Troubleshooting & Failure Log
- **What broke:** (e.g., *Navigated to the public IP address in Chrome, but the connection timed out.*)
- **How I diagnosed it:** (e.g., *Checked the URL and noticed Chrome forced `https://`. Also checked Security Group inbound rules.*)
- **How I fixed it:** (e.g., *Changed the URL to `http://` explicitly, and confirmed Port 80 was open to `0.0.0.0/0` in the inbound rules.*)

---

## Verification & Screenshots
| Running Instance (2/2 Passed) | Security Group Rules | Live Apache Webpage |
| :---: | :---: | :---: |
| ![Instance](images/ec2-running.png) | ![Security Group](images/ec2-sg.png) | ![Webpage](images/ec2-webpage.png) |

---

## Day 4 Interview Questions & Answers

### 1. What is the difference between an EC2 Security Group and a Network ACL (NACL)?
**Answer:** A Security Group operates at the instance/ENI level and is **stateful** (if inbound traffic is allowed, response traffic is automatically permitted). A Network ACL operates at the subnet level and is **stateless** (rules must be explicitly defined for both inbound and outbound traffic).

### 2. What happens to data stored on an EC2 instance when it is stopped vs. terminated?
**Answer:** If the instance uses an EBS root volume, stopping the instance preserves the data on EBS (you only pay for EBS storage, not compute). If the instance is terminated, the root EBS volume is deleted by default (unless *DeleteOnTermination* is unchecked), and any ephemeral Instance Store data is permanently lost.

### 3. Why should Port 22 (SSH) never be set to `0.0.0.0/0`?
**Answer:** Setting CIDR `0.0.0.0/0` exposes the SSH management port to the entire public internet, opening the instance to constant automated brute-force password and key attacks. SSH access should always be restricted to a specific IP address, CIDR block, or bastion host.

### 4. What is the role of EC2 User Data?
**Answer:** User Data allows you to specify a bootstrap script (such as bash or cloud-init directives) that executes automatically with root privileges during the first boot cycle of the instance to automate configuration, updates, and package installations.

---

## Cleanup & Safety Verification
- [x] **Terminated the EC2 instance** to prevent any accidental Free Tier usage overrun.
- [x] Confirmed the attached root EBS volume was automatically deleted.
- [x] Confirmed zero billable resources running.
