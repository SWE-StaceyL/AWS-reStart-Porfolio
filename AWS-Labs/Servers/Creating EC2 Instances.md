# AWS EC2 Instance Creation Lab via is CLI

---

## Overview

This project documents the successful completion of an AWS hands-on lab focused on launching and configuring Amazon EC2 instances using two methods: the **AWS Management Console** and the **AWS Command Line Interface (AWS CLI)**. A bastion host architecture was implemented to securely access and manage a web server instance within a VPC.

---

## Architecture

<img width="1610" height="724" alt="Screenshot 2026-03-18 140604" src="https://github.com/user-attachments/assets/b811a245-8ef3-49e8-ab94-c8faff827b9b" />

## Objectives Achieved

- [x] Launched an EC2 instance (Bastion Host) using the **AWS Management Console**
- [x] Connected to the instance securely using **EC2 Instance Connect**
- [x] Launched a second EC2 instance (Web Server) using the **AWS CLI**
- [x] Retrieved dynamic environment values (AMI ID, Subnet ID, Security Group ID) via CLI
- [x] Deployed a web application using a **User Data script** on instance launch
- [x] Verified the web server was live via public DNS

## AWS Services Used

| Service | Purpose |
|---|---|
| **Amazon EC2** | Compute — Bastion Host & Web Server instances |
| **Amazon VPC** | Isolated network environment with public subnet |
| **EC2 Instance Connect** | Secure, browser-based SSH access |
| **AWS Systems Manager (SSM)** | Retrieved the latest Amazon Linux 2 AMI ID from Parameter Store |
| **IAM** | Instance profile (`Bastion-Role`) granting EC2 permissions |
| **Security Groups** | Firewall rules for SSH (port 22) and HTTP (port 80) |


## Instance Configuration

### Bastion Host
| Setting | Value |
|---|---|
| Name | `Bastion host` |
| AMI | Amazon Linux 2 (HVM) |
| Instance Type | `t3.micro` |
| Key Pair | None (EC2 Instance Connect used) |
| Subnet | Public Subnet — Lab VPC |
| Security Group | `Bastion security group` — SSH permitted |
| IAM Profile | `Bastion-Role` |

### Web Server
| Setting | Value |
|---|---|
| Name | `Web Server` |
| AMI | Latest Amazon Linux 2 (retrieved via SSM Parameter Store) |
| Instance Type | `t3.micro` |
| Subnet | Public Subnet — Lab VPC |
| Security Group | `WebSecurityGroup` — HTTP permitted |
| User Data | Apache web server + web application installation script |

<img width="1911" height="994" alt="Screenshot 2026-03-18 141902" src="https://github.com/user-attachments/assets/9b09401a-bb4b-4b53-b886-76bd00c18bc4" />

<img width="1896" height="961" alt="Screenshot 2026-03-18 141946" src="https://github.com/user-attachments/assets/d5632981-b1a4-47ae-b25e-a3252ea3263b" />


## Key CLI Commands

### Retrieve the latest Amazon Linux 2 AMI
bash
AZ=`curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone`
export AWS_DEFAULT_REGION=${AZ::-1}

AMI=$(aws ssm get-parameters \
  --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 \
  --query 'Parameters[0].[Value]' \
  --output text)

<img width="1909" height="929" alt="Screenshot 2026-03-18 142138" src="https://github.com/user-attachments/assets/34f232d6-56c4-46c4-be80-99eca2109299" />

### Retrieve Subnet and Security Group IDs
bash
SUBNET=$(aws ec2 describe-subnets \
  --filters 'Name=tag:Name,Values=Public Subnet' \
  --query Subnets[].SubnetId \
  --output text)

<img width="1903" height="961" alt="Screenshot 2026-03-18 142225" src="https://github.com/user-attachments/assets/0c9f161e-46fe-4f4a-b641-88dcdc4923ed" />


SG=$(aws ec2 describe-security-groups \
  --filters Name=group-name,Values=WebSecurityGroup \
  --query SecurityGroups[].GroupId \
  --output text)

<img width="1905" height="960" alt="Screenshot 2026-03-18 142258" src="https://github.com/user-attachments/assets/6c10012d-4376-405a-b582-cd5fbbb3c949" />


### Launch the Web Server instance
bash
INSTANCE=$(aws ec2 run-instances \
  --image-id $AMI \
  --subnet-id $SUBNET \
  --security-group-ids $SG \
  --user-data file:///home/ec2-user/UserData.txt \
  --instance-type t3.micro \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=Web Server}]' \
  --query 'Instances[*].InstanceId' \
  --output text)

### Check instance state
bash
aws ec2 describe-instances \
  --instance-ids $INSTANCE \
  --query 'Reservations[].Instances[].State.Name' \
  --output text

<img width="1909" height="930" alt="Screenshot 2026-03-18 142423" src="https://github.com/user-attachments/assets/88933991-e24b-4d64-b323-9e0fb9b78884" />


### Retrieve public DNS name
bash
aws ec2 describe-instances \
  --instance-ids $INSTANCE \
  --query Reservations[].Instances[].PublicDnsName \
  --output text

<img width="1904" height="951" alt="Screenshot 2026-03-18 142547" src="https://github.com/user-attachments/assets/5505bd09-7cfb-4c38-853a-138974c5723a" />


## Key Learnings

- **Bastion host pattern** — A bastion host acts as a secure jump server, allowing controlled access to resources within a private network without exposing them directly to the internet.
- **Instance metadata service** — EC2 instances can query `http://169.254.169.254` to retrieve runtime information such as their Availability Zone, which can then be used to dynamically set the AWS Region in CLI commands.
- **SSM Parameter Store** — AWS maintains a public parameter path (`/aws/service/ami-amazon-linux-latest/`) that always returns the latest AMI IDs, enabling automation scripts to stay current without hardcoding AMI values.
- **User Data scripts** — Commands passed via User Data run automatically at first boot, enabling zero-touch configuration of instances at launch time.
- **Console vs CLI** — The Management Console is best for one-off or exploratory tasks; the CLI is preferred for repeatable, automated, and auditable deployments.

## Programme Context

**Programme:** AWS re/Start  
**Provider:** Praesignis  
**Location:** Johannesburg, South Africa  
**Goal:** AWS Certified Cloud Practitioner (CLF-C02)

## Conclusion

This lab provided practical, hands-on experience with two of the most common methods used to provision compute resources on AWS — the Management Console and the AWS CLI.
By launching a Bastion Host through the console and using it as a secure gateway to deploy a Web Server via CLI commands, the lab demonstrated a realistic, layered architecture that mirrors real-world cloud engineering practices. Key concepts reinforced throughout include the role of security groups as virtual firewalls, the use of IAM instance profiles to grant scoped permissions to EC2 instances, and the power of User Data scripts to automate instance configuration at boot time.
The CLI portion of the lab highlighted how cloud infrastructure can be provisioned in a repeatable and automated manner — retrieving dynamic values such as AMI IDs, Subnet IDs, and Security Group IDs programmatically, rather than hardcoding them. This approach reduces human error and lays the groundwork for more advanced automation using tools like AWS CloudFormation or scripted deployment pipelines.
Overall, this lab reinforced that understanding when and how to use each provisioning method is a foundational skill for any Cloud Engineer or Solutions Architect — and that the AWS CLI is an indispensable tool for building reliable, scalable infrastructure efficiently.



