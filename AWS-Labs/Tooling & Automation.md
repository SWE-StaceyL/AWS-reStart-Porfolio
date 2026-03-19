# 🖥️ AWS Systems Manager Lab

**CloudLearners Inc. | AWS re/Start Programme**

---

## 📌 Overview

This lab demonstrates how to use **AWS Systems Manager (SSM)** to manage and interact with EC2 instances — without requiring direct SSH access or open inbound ports. It covers two core SSM capabilities: **Run Command** and **Session Manager**.

---

## 🎯 Objectives

By completing this lab, the following was achieved:

- ✅ Used **Run Command** to remotely install and configure a web application on an EC2 instance
- ✅ Accessed an EC2 instance securely via **Session Manager** — no SSH required
- ✅ Verified installed application files using Linux commands in an SSM session
- ✅ Queried EC2 instance metadata and described instances using the AWS CLI

---

## 🛠️ Services & Tools Used

| Service / Tool | Purpose |
|---|---|
| AWS Systems Manager | Central management of EC2 instances |
| SSM Run Command | Remote command execution on managed instances |
| SSM Session Manager | Secure browser-based shell access to EC2 |
| Amazon EC2 | Target compute instance (Managed Instance) |
| AWS IAM | Permissions and access control for SSM |
| AWS CloudTrail | Auditing and logging of Session Manager activity |
| AWS CLI | Querying EC2 instance details in JSON format |

---

## 🔄 Architecture

```
AWS Systems Manager
       │
       ├── Run Command ──────► EC2 Managed Instance (VPC)
       │                            └── Installs: Apache, PHP, AWS SDK, Web App
       │
       └── Session Manager ──► EC2 Managed Instance (VPC)
                                    └── Secure shell access (no SSH / port 22)
```

---

## 🧪 Lab Tasks

### Task 1 — Install Application Using Run Command

1. Navigated to **Node Management → Run Command** in the SSM console
2. Filtered documents by **Owner: Owned by me** to locate the custom `Install Dashboard App` document
3. Configured the run command:
   - **Document version:** 1 (Default)
   - **Target selection:** Choose instances manually → Selected **Managed Instance**
   - **Output options:** Disabled S3 bucket logging
4. Executed the command, which installed:
   - Apache Web Server
   - PHP
   - AWS SDK
   - Web Application
   - Started the web server automatically

---

### Task 2 — Access EC2 via Session Manager

1. Navigated to **Node Management → Session Manager**
2. Started a new session on the **Managed Instance**
3. Ran the following commands in the session window:

```bash
# List installed application files
ls /var/www/html

# Retrieve the instance's availability zone and set as default region
AZ=`curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone`
export AWS_DEFAULT_REGION=${AZ::-1}

# Describe EC2 instances (output in JSON)
aws ec2 describe-instances
```

> 💡 Session Manager provides secure access without requiring an open SSH port (port 22), making it more secure than traditional SSH access.

---

## 🔐 Security Highlights

- Session Manager access is controlled via **IAM policies**
- All session activity is logged through **AWS CloudTrail**
- The EC2 instance's security group had **port 22 (SSH) closed**, confirming that SSM is the secure access path
- IAM permissions were required for the SSM Agent to register and interact with the Systems Manager service

---

## 💡 Key Takeaways

- **SSM Run Command** enables remote, automated management of instances at scale — including targeting entire fleets using **tags**
- **Session Manager** eliminates the need for SSH keys and open inbound ports, reducing the attack surface
- The **SSM Agent** must be installed and registered on an instance for it to appear as a **Managed Instance**
- Systems Manager integrates with IAM and CloudTrail for auditing, making it enterprise-ready

---

## 📁 Repository Structure

```
├── README.md          # Project overview and documentation
```

---

## 👩‍💻 Author

**Stacey** | AWS re/Start Learner | Cloud & Cybersecurity Enthusiast
GitHub: [@SWE-StaceyL](https://github.com/SWE-StaceyL)

---

*Part of the CloudLearners Inc. project portfolio — AWS re/Start Programme, Johannesburg*
