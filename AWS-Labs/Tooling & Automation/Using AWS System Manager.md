# AWS Systems Manager Lab



---

## Overview

This lab demonstrates how to use **AWS Systems Manager (SSM)** to manage and interact with EC2 instances — without requiring direct SSH access or open inbound ports. It covers two core SSM capabilities: **Run Command** and **Session Manager**.

---

## Objectives

By completing this lab, the following was achieved:

- ✅ Used **Run Command** to remotely install and configure a web application on an EC2 instance
- ✅ Accessed an EC2 instance securely via **Session Manager** — no SSH required
- ✅ Verified installed application files using Linux commands in an SSM session
- ✅ Queried EC2 instance metadata and described instances using the AWS CLI

---

## Services & Tools Used

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

## Architecture

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

##  Lab Tasks

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
   - 
<img width="1914" height="977" alt="Screenshot 2026-03-17 182857" src="https://github.com/user-attachments/assets/14757e7b-5a29-4439-ac62-ddab76145585" />

<img width="1911" height="922" alt="Screenshot 2026-03-17 183043" src="https://github.com/user-attachments/assets/7b28e772-7509-4423-9639-58e71c248253" />

<img width="1912" height="916" alt="Screenshot 2026-03-17 183556" src="https://github.com/user-attachments/assets/d25c5a21-abf1-4a4a-9d9c-575c7653cb3c" />

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

Session Manager provides secure access without requiring an open SSH port (port 22), making it more secure than traditional SSH access.

<img width="1907" height="957" alt="Screenshot 2026-03-17 184948" src="https://github.com/user-attachments/assets/3c3a9f83-54e3-429c-801b-3fe2b3474742" />

<img width="1915" height="973" alt="Screenshot 2026-03-17 185016" src="https://github.com/user-attachments/assets/a8e3e502-244b-4571-9eb7-bde15ad4497e" />

---

## Security Highlights

- Session Manager access is controlled via **IAM policies**
- All session activity is logged through **AWS CloudTrail**
- The EC2 instance's security group had **port 22 (SSH) closed**, confirming that SSM is the secure access path
- IAM permissions were required for the SSM Agent to register and interact with the Systems Manager service

<img width="1918" height="998" alt="Screenshot 2026-03-17 184237" src="https://github.com/user-attachments/assets/f80de130-e26f-40d4-a4fc-bfba90a6d73d" />

<img width="1916" height="996" alt="Screenshot 2026-03-17 184703" src="https://github.com/user-attachments/assets/de4fb4d3-72f2-4584-aefb-330ef3d6d803" />

---

## Key Takeaways

- **SSM Run Command** enables remote, automated management of instances at scale — including targeting entire fleets using **tags**
- **Session Manager** eliminates the need for SSH keys and open inbound ports, reducing the attack surface
- The **SSM Agent** must be installed and registered on an instance for it to appear as a **Managed Instance**
- Systems Manager integrates with IAM and CloudTrail for auditing, making it enterprise-ready

---

## Consulsion

This lab provided hands-on experience with two of AWS Systems Manager's most powerful node management capabilities. By using Run Command, a full web application stack was deployed to an EC2 instance remotely and at scale — without ever logging into the machine directly. Through Session Manager, secure shell access was established without SSH keys or open inbound ports, demonstrating a modern, auditable alternative to traditional instance access.
Together, these tools highlight how AWS Systems Manager enables cloud administrators to manage infrastructure securely, efficiently, and at scale — core principles of both cloud engineering and cybersecurity best practice. This lab reinforces the value of centralised instance management as part of a well-architected AWS environment.




---

*Part of the CloudLearners Inc. project portfolio — AWS re/Start Programme, Johannesburg*
