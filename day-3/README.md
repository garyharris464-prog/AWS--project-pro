
 Day 3: Amazon S3 — Object Storage & Static Website Hosting

**Date:** [11/03/2026]  
**Status:** Completed  

---

## Overview & What I Learned
- **Object Storage vs. Block/File Storage:** S3 stores data as individual objects (data + metadata + unique key) within flat buckets, rather than a hierarchical operating system drive (EBS) or shared file system (EFS).
- **Global Namespace, Regional Confinement:** Bucket names are globally unique across all AWS accounts worldwide, but data resides in the chosen AWS .
- **S3 Security Layers:** Block Public Access settings act as a master safety lock overriding resource policies. Static hosting requires specific `s3:GetObject` allowances.
- **Durability & Availability:** Standard S3 provides 99.999999999% (11 9s) of durability by automatically replicating data across multiple Availability Zones
---

## What I Built / Configured
1. Provisioned a globally unique Amazon S3 bucket in `us-east-1`.
2. Configured S3 Static Website Hosting with `index.html` as the root entry point.
3. Configured Block Public Access settings and applied an explicit JSON bucket policy allowing `s3:GetObject`.
4. Uploaded website assets and verified public HTTP endpoint resolution.

---

## Troubleshooting & Failure Log
- **What broke:** (e.g., *Received `403 Forbidden` error when loading the S3 website endpoint.*)
- **How I diagnosed it:** (e.g., *Checked bucket permissions and saw the JSON bucket policy had a syntax error in the ARN resource field.*)
- **How I fixed it:** (e.g., *Appended `/*` to the ARN so the policy applied to every object inside the bucket rather than just the bucket container itself.*)

---

## Verification & Screenshots
| S3 Bucket Properties | Bucket Policy Configured | Live Site Verification |
| :---: | :---: | :---: |
| ![Bucket](images/s3-bucket.png) | ![Policy](images/s3-policy.png) | ![Live Site](images/s3-site.png) |

---

## Day 3 Interview Questions & Answers

### 1. What makes an S3 bucket name unique?
**Answer:** The namespace is global across all AWS accounts worldwide. Once an account claims a bucket name in any region, no other AWS account can use that name until it is deleted.

### 2. What is the difference between S3 Block Public Access and a Bucket Policy?
**Answer:** Block Public Access is a centralized safeguard/firewall that overrides all other permissions. If Block Public Access is turned on, even a bucket policy explicitly granting `Allow` to public principals (`*`) will be ignored and traffic will be denied.

### 3. What does 11 9s of durability (99.999999999%) actually mean?
**Answer:** Durability measures the likelihood of data loss or corruption, not uptime. 11 9s means that if you store 10,000,000 objects in S3, on average you would lose one object every 10,000 years, achieved by redundant storage across at least three distinct Availability Zones.
### 4. Why does the S3 bucket policy Resource line require `/*` for `s3:GetObject`?
**Answer:** The action `s3:GetObject` applies to individual objects, not the bucket container. Writing `arn:aws:s3:::my-bucket` references the container; `arn:aws:s3:::my-bucket/*` references all objects residing within that container.

---

## Cleanup & Safety Verification
- [x] Deleted demo static site objects (if not keeping live for portfolio).
- [x] Verified zero running compute or database charges).
