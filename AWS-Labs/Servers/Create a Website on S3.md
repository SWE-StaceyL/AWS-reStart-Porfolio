# Creating a Static Website on Amazon S3 through the CLI

 **AWS re/Start Programme |**
 Deploying a static website using Amazon S3, IAM, and the AWS CLI from an EC2 instance.

## Project Overview

This lab demonstrates how to host a static website on **Amazon Simple Storage Service (Amazon S3)** using the **AWS Command Line Interface (AWS CLI)** from an Amazon EC2 instance. The project simulates a real-world scenario for a fictional client, **Café & Bakery**, requiring a cost-effective, scalable, and publicly accessible web presence.

The core tasks include creating an S3 bucket, configuring IAM permissions, uploading website assets, enabling static website hosting, and writing an automation script for future updates.

| Detail | Value |
|---|---|
| **Client** | Café & Bakery (CloudLearners Inc.) |
| **Programme** | AWS re/Start |
| **Lab Duration** | ~45 minutes |
| **AWS Region** | `us-west-2` (US West — Oregon) |
| **Hosting Method** | Amazon S3 Static Website Hosting |

---

## Architecture Diagram

<img width="1258" height="575" alt="Screenshot 2026-03-18 132206" src="https://github.com/user-attachments/assets/7079e6f0-82d8-4faf-9a23-6db04ed330b6" />

## Technologies Used

| Technology | Purpose |
|---|---|
| **Amazon S3** | Static website hosting and file storage |
| **Amazon EC2** | CLI execution environment |

| **AWS IAM** | User creation and permission management |
| **AWS CLI** | Infrastructure provisioning via command line |
| **AWS Systems Manager (SSM)** | Secure browser-based terminal access |
| **HTML / CSS** | Static website front-end assets |
| **Bash** | Automation script for website updates |

## Prerequisites

Before starting this lab, ensure you have:

- [ ] Access to an **AWS Academy / Vocareum** lab environment
- [ ] An active lab session with **Lab Status: Ready**
- [ ] Lab credentials (AccessKey + SecretKey) from the **Details → AWS CLI** panel
- [ ] Basic understanding of the **AWS CLI** and **Linux command line**
- [ ] Familiarity with **IAM users and policies**

> ⚠️ **Important:** This lab uses temporary lab credentials. Always retrieve your `AccessKey` and `SecretKey` from the lab **Credentials panel** — do not use personal AWS account credentials.

---

## Step-by-Step Implementation

### Phase 1: Environment Setup & CLI Authentication

I Connected to the EC2 instance via **AWS Systems Manager Session Manager** and configure the AWS CLI with lab credentials.

 bash
 Verify AWS CLI installation
 aws --version

Switch to ec2-user where lab files are staged
sudo su - ec2-user

Configure CLI with lab credentials
aws configure
 AWS Access Key ID: [YOUR-ACCESS-KEY-FROM-LAB-PANEL]
 AWS Secret Access Key: [YOUR-SECRET-KEY-FROM-LAB-PANEL]
 Default region name: us-west-2
 Default output format: json

Clear any stale session tokens (important in lab environments)
printf '[default]\naws_access_key_id = YOUR-ACCESS-KEY\naws_secret_access_key = YOUR-SECRET-KEY\n' > ~/.aws/credentials

Verify authentication
aws sts get-caller-identity

<img width="1916" height="978" alt="Screenshot 2026-03-18 115314" src="https://github.com/user-attachments/assets/1cdb462c-a61f-4131-ba74-e4b6ee9c25da" />

**Expected output:**

json
{
    "Account": "XXXXXXXXXXXX",
    "UserId": "AIDAXXXXXXXXXXXX",
    "Arn": "arn:aws:iam::XXXXXXXXXXXX:user/awsstudent"
}


<img width="1902" height="930" alt="Screenshot 2026-03-18 123027" src="https://github.com/user-attachments/assets/9a161649-c780-423b-a8fc-0b3c370b7e95" />

Phase 2: Create S3 Bucket

bash
Create a globally unique S3 bucket
aws s3 mb s3://[YOUR-UNIQUE-BUCKET-NAME]

Example:
aws s3 mb s3://cafe-website-stacey-2026

Verify bucket creation
aws s3 ls

**Bucket naming rules:** Must be globally unique, lowercase, 3–63 characters, no underscores.

Phase 3: Create IAM User with S3 Access

bash
Create a new IAM user
aws iam create-user --user-name awsS3user

Attach AmazonS3FullAccess managed policy
aws iam attach-user-policy \
  --user-name awsS3user \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess

Verify the policy was attached
aws iam list-attached-user-policies --user-name awsS3user

Phase 4: Upload Website Files

bash
Extract the website archive
cd ~/sysops-activity-files/
tar -xzf *.tar.gz

Navigate to the website directory
cd static-website
ls   Confirm index.html, css/, images/ are present

Upload all files recursively to S3
aws s3 cp . s3://[YOUR-BUCKET-NAME]/ --recursive

Verify files uploaded correctly
aws s3 ls s3://[YOUR-BUCKET-NAME]/

**Expected files in bucket:**

index.html
css/styles.css
images/Coffee-Shop.png
images/Cafe-Vitrine.png
images/Cookies.png
images/Coffee-and-Pastries.png
images/Strawberry-Tarts.png

Phase 5: Configure Static Website Hosting

This phase is completed via the **AWS Management Console** due to IAM permission constraints on the EC2 instance role.

**Step 1 — Disable Block Public Access:**
1. Navigate to **S3 → [Your Bucket] → Permissions tab**
2. Click **Block public access (bucket settings) → Edit**
3. Uncheck **all four boxes**
4. Click **Save changes** → type `confirm` → **Confirm**

**Step 2 — Apply Public Bucket Policy:**
1. Still in **Permissions tab → Bucket policy → Edit**
2. Paste the following policy:

json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::[YOUR-BUCKET-NAME]/*"
    }
  ]
}

3. Click **Save changes**

**Step 3 — Enable Static Website Hosting:**
1. Go to **Properties tab → Static website hosting → Edit**
2. Select **Enable**
3. Set **Index document:** `index.html`
4. Click **Save changes**

 Phase 6: Create the Update Script

bash
 Create the update script
cat > ~/update-website.sh << 'EOF'
!/bin/bash
 update-website.sh
 Syncs local website files to the S3 bucket
 Usage: ./update-website.sh

BUCKET_NAME="[YOUR-BUCKET-NAME]"
LOCAL_PATH="$HOME/sysops-activity-files/static-website"

echo "Uploading website files to s3://$BUCKET_NAME ..."
aws s3 cp "$LOCAL_PATH" "s3://$BUCKET_NAME/" --recursive
echo  Website updated successfully!"
echo "URL: http://$BUCKET_NAME.s3-website-us-west-2.amazonaws.com"
EOF

Make executable
chmod +x ~/update-website.sh

Test the script
~/update-website.sh

## Challenges & Troubleshooting

This lab involved several real-world troubleshooting scenarios. The table below documents each issue and its resolution.

|  | Issue Encountered | Root Cause | Resolution |
|---|---|---|---|
| 1 | `AccessDenied` on `s3:CreateBucket` | CLI not yet configured — defaulting to EC2 instance role with no S3 permissions | Configured AWS CLI with lab `AccessKey` and `SecretKey` from the Credentials panel |
| 2 | `InvalidClientTokenId` after configuring CLI | A Session Manager URL was accidentally pasted as the `aws_session_token` value | Overwrote `~/.aws/credentials` using `printf` to write a clean file with no session token |
| 3 | `aws s3 cp` uploaded wrong files (system binaries) | Command was run from `/usr/bin` instead of the website directory | Identified working directory with `pwd`; cleaned bucket with `aws s3 rm --recursive`; navigated to correct path |
| 4 | `Permission denied` accessing `/home/ec2-user/` | Session Manager connects as `ssm-user`, not `ec2-user` | Used `sudo su - ec2-user` to switch users; re-configured AWS CLI under `ec2-user`'s profile |
| 5 | `tar` extraction failed — `No such file or directory` | File was named `static-website-v2.tar.gz`, not `v3` as assumed | Used `tar -xzf *.tar.gz` wildcard to extract regardless of version number |
| 6 | `AccessDenied` on `s3:PutBucketPolicy` via Console | Block Public Access was still enabled, conflicting with the public bucket policy | Disabled all Block Public Access settings first, then applied the bucket policy |

---

## Key Concepts Learned

**Amazon S3 Static Website Hosting**
S3 can serve static HTML, CSS, and image files directly over HTTP without requiring a web server. The bucket must have public access enabled and a bucket policy granting `s3:GetObject` to all principals.

<img width="1902" height="912" alt="Screenshot 2026-03-18 121943" src="https://github.com/user-attachments/assets/730e6358-bafe-443b-8c96-5089ed5aa10a" />

**IAM Credential Scoping**
In Linux, AWS CLI credentials are stored per-user in `~/.aws/credentials`. Credentials configured as `ssm-user` are not available to `ec2-user` and vice versa — each user has an independent credential store.

**EC2 Instance Roles vs. IAM User Credentials**
EC2 instances are assigned IAM roles that define what AWS actions the instance itself can perform. These role permissions are separate from and may be more restrictive than the lab IAM user credentials. Always verify which identity is active using `aws sts get-caller-identity`.

**S3 Block Public Access**
AWS applies Block Public Access settings as a guardrail against accidental data exposure. For public static websites, all four Block Public Access settings must be disabled at the bucket level before a public bucket policy can take effect.

<img width="1907" height="933" alt="Screenshot 2026-03-18 122947" src="https://github.com/user-attachments/assets/b06e400d-71f3-4cc5-8c77-e9263b7f0550" />

**Bash Automation Scripts**
Repeatable AWS CLI operations can be wrapped in Bash scripts to enable one-command deployments, reducing human error and supporting CI/CD-style workflows even for simple static sites.

Replace the placeholders below with actual screenshots from your lab.

| Screenshot | Description |
|---|---|
| `screenshots/01-bucket-created.png` | Output of `aws s3 ls` showing the newly created bucket |
| `screenshots/02-iam-user-created.png` | JSON response from `aws iam create-user` |
| `screenshots/03-files-uploaded.png` | Terminal output showing website files uploading to S3 |
| `screenshots/04-bucket-policy.png` | AWS Console showing the public bucket policy applied |
| `screenshots/05-website-hosting-enabled.png` | S3 Properties tab showing static website hosting enabled |
| `screenshots/06-live-website.png` | Browser showing the live Café & Bakery website |

 Live Website

**Website URL:**
```
http://[YOUR-BUCKET-NAME].s3-website-us-west-2.amazonaws.com
```
**Example:**
```
http://cafe-website-stacey-2026.s3-website-us-west-2.amazonaws.com
```
<img width="1901" height="965" alt="Screenshot 2026-03-18 123144" src="https://github.com/user-attachments/assets/9bc0cb8a-7fa8-4a6a-8283-c866eba58251" />

## Learning Objectives

After completing this lab, you will be able to:

✅ Authenticate the AWS CLI using IAM credentials from within an EC2 instance  
✅ Create and configure an Amazon S3 bucket via the AWS CLI  
✅ Create an IAM user and attach managed policies programmatically  
✅ Upload a static website to S3 using recursive copy commands  
✅ Enable S3 static website hosting and configure a public bucket policy  
✅ Write a reusable Bash script to automate future website deployments  

## Conclusion

This lab provided hands-on experience deploying a static website to Amazon S3 
using the AWS CLI from an EC2 instance — simulating a real-world cloud deployment 
workflow from end to end.

Beyond the core technical objectives, this lab presented several unexpected 
troubleshooting scenarios that deepened my understanding of how AWS identity, 
credentials, and permissions interact in practice. Working through issues such as 
stale session tokens, user context switching between `ssm-user` and `ec2-user`, 
incorrect working directories, and the ordering dependency between Block Public 
Access settings and bucket policies reinforced a critical cloud engineering mindset: 
**understanding not just how to run commands, but why they succeed or fail.**

Key takeaways from this lab include:

- **IAM credentials are scoped per Linux user** — configuring the CLI as one user 
  does not carry over to another, a subtle but important detail in multi-user 
  environments.
- **The order of AWS operations matters** — attempting to apply a public bucket 
  policy before disabling Block Public Access will always fail, regardless of the 
  policy's correctness.
- **Real cloud work involves debugging** — the ability to read error messages, 
  identify the failing API call, and trace it back to a missing permission or 
  misconfiguration is as valuable as knowing the commands themselves.
- **S3 is a powerful, low-cost hosting solution** — for static content, S3 
  eliminates the need for a web server entirely, reducing operational overhead 
  and cost while maintaining scalability and availability.

This project is part of my growing AWS portfolio built during the 
**Praesignis AWS re/Start Programme**, and reflects my commitment to developing 
practical, job-ready cloud skills across infrastructure, security, and automation.

> *This project was completed as part of the Praesignis AWS re/Start Programme and is part of the CloudLearners Inc. portfolio series.*
