
# Day 2: IAM Deep Dive — Groups, Policies & Delegated Access

**Date:** [11/2/2026]  
**Status:** Completed  

---

## Overview & What I Learned
- **Principle of Least Privilege:** Users should only be granted the minimum permissions necessary to complete their job duties.
- **Users vs. Groups vs. Roles:**
  - **User:** An individual identity (person or application).
  - **Group:** A collection of users used to attach permissions to multiple people at once.
  - **Role:** An identity with temporary permissions assumed by users, applications, or AWS services (no long-term passwords).
- **Policy Structure:** IAM policies are JSON documents built around Effect, Action, Resource, and Condition (`PARC`).

---

## What I Built / Configured
1. Created an IAM User Group named `CloudAdmins`.
2. Attached the AWS-managed policy `AdministratorAccess` to `CloudAdmins`.
3. Created a daily admin IAM user (`cloud-admin-user`) and added them to the group.
4. Enforced MFA and custom password rotation on first sign-in.
5. Successfully tested console login using the unique account alias URL.

---

## Troubleshooting & Failure Log
- **What broke:** (e.g., *Tried logging into the standard console without the 12-digit Account ID and was rejected.*)
- **How I diagnosed it:** (e.g., *IAM users require either the 12-digit Account ID or the custom account alias to authenticate.*)
- **How I fixed it:** (e.g., *Bookmarked the specific AWS Sign-in URL with the account alias pre-filled.*)

---

## Verification & Screenshots
| User Group & Policy | User Assigned | MFA Active |
| :---: | :---: | :---: |
| ![Group](images/iam-group.png) | ![User](images/iam-user.png) | ![MFA](images/user-mfa.png) |

---

## Day 2 Interview Questions & Answers

### 1. What is the difference between an IAM User and an IAM Role?
**Answer:** An IAM user has permanent credentials (username/password or access keys) representing a single person or service. An IAM role does not have permanent credentials; it is temporarily assumed by trusted entities (like EC2 instances, Lambda functions, or cross-account users) using temporary security tokens via AWS STS.

### 2. Why should you assign policies to Groups rather than individual Users?
**Answer:** Group-based access simplifies governance and scaling. Attaching policies to individual users leads to permission drift and makes auditing difficult. Managing permissions at the group level ensures consistent access when team members join, move, or leave.

### 3. What is an IAM Policy and what are its core JSON elements?
**Answer:** An IAM policy is a JSON document that defines permissions. Its core elements are:
- **Effect:** `Allow` or `Deny`.
- **Action:** The API calls being permitted/restricted (e.g., `s3:GetObject`).
- **Resource:** The ARN of the specific resource the action applies to.
- **Condition:** Optional criteria under which the policy is valid (e.g., requiring MFA or an IP range).

### 4. If an explicit Allow and an explicit Deny conflict in IAM, which wins?
**Answer:** An explicit `Deny` always overrides an `Allow`. If no explicit Allow exists, the request defaults to an implicit deny.

---

## Cleanup & Safety Verification
- [x] Tested login with `cloud-admin-user` and verified admin rights.
- [x] MFA configured and validated.
- [x] Root account credentials stored in a password manager and logged out.
- [x] Zero billable resources running.
