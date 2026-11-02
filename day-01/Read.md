# Day 1: AWS Foundations — Account Safety, Console, Regions & AZs

**Date:** [11/1/2026]  
**Status:** Completed  

---

## Overview & What I Learned
- **Root Account vs. IAM User:** Why the root account should be secured and locked away after creating an administrative IAM user.
- **Regions & Availability Zones (AZs):** A Region is an isolated geographic location containing multiple isolated data center clusters called Availability Zones (AZs).
- **Billing Protection:** Setting up proactive budget alarms before provisioning resources to prevent surprise bills.

---

## What I Built / Configured
1. **Root Account Security:** Enabled Multi-Factor Authentication (MFA) on the root account.
2. **IAM Administrative User:** Created an everyday IAM user to replace root account usage for daily lab work.
3. **Region Configuration:** Set default lab working region to `us-east-1` (N. Virginia).
4. **Zero-Spend / CloudWatch Billing Alert:** Created a budget alert threshold (e.g., $10) via AWS Billing & Cost Management.

---

## Troubleshooting & Failure Log
- **What broke:** (e.g., *Console showed 'Access Denied' when navigating to Billing from the new IAM user.*)
- **How I diagnosed it:** (e.g., *Checked IAM user permissions and noticed IAM user access to billing wasn't activated at the root level.*)
- **How I fixed it:** (e.g., *Signed back into Root > Account Settings > IAM User and Role Access to Billing Information > Activated access.*)

---

## Verification & Screenshots
| MFA Enabled | IAM User Created | Billing Budget |
| :---: | :---: | :---: |
| ![MFA](images/mfa-setup.png) | ![IAM](images/iam-user.png) | ![Billing](images/billing-alarm.png) |

---

## Day 1 Interview Questions & Answers

### 1. Why shouldn't the AWS root user be used for everyday work?
**Answer:** The root user has complete, unrestricted access across all resources and billing. If compromised, an attacker has total control. Using an IAM user adheres to the principle of least privilege.

### 2. What is an AWS Region?
**Answer:** A physical geographic location in the world containing multiple, physically separated and isolated Availability Zones connected by low-latency networking

### 3. What is an Availability Zone (AZ)?
**Answer:** One or more discrete data centers with redundant power, networking, and connectivity within an AWS Region.

### 4. Why can an EC2 resource appear to disappear when you change Regions?
**Answer:** Most AWS resources (like EC2 and VPC) are regional. The AWS Console only displays resources existing within the currently selected Region.

### 5. What problem does AWS IAM solve?
**Answer:** It provides centralized authentication and authorization, allowing you to control who can sign in (authentication) and what specific actions they are permitted to perform (authorization)

---

## Cleanup & Safety Verification
- [x] No billable compute (EC2), database (RDS), or NAT Gateways left running
- [x] Root MFA remains enabled
- [x] Budget alert active.
