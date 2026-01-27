AWS EC2 Hands-On Lab

<img width="400" height="341" alt="image 1" src="https://github.com/user-attachments/assets/1b4a4c7e-7a53-4e29-b672-9670d42b797b" />

### Overview
This lab demonstrates practical experience with Amazon EC2 (Elastic Compute Cloud), a core AWS service that provides scalable computing capacity in the cloud. During this lab, I worked through the process of launching, configuring, securing, monitoring, and scaling EC2 instances, gaining a clear understanding of how cloud infrastructure behaves in real-world scenarios.

By completing this lab, I not only deployed a working web server but also explored security, automation, and instance lifecycle management, which are critical for building reliable and resilient cloud applications.

### Lab Architecture
The lab involved deploying a web server on EC2 with proper security and scalability:


### Lab Work

Task 1️: Launching EC2 Instance
I began by navigating my way on the management console to EC2 and then launching an EC2 instance named Web Server using the Amazon Linux 2023 AMI.  It reached runniing state and passed 2/2 status checks. I selected a t3.micro instance type and configured termination protection to prevent accidental deletion deployed a simple web page.

### Instance Configuration
<img width="1500" height="341" alt="Screenshot 2026-01-26 142635" src="https://github.com/user-attachments/assets/93493da9-63e0-45d2-8ecd-6d76abdacd4e" />

### Selected t3.micro for Instance Type

<img width="1500" height="341" alt="Screenshot 2026-01-26 143739" src="https://github.com/user-attachments/assets/d757a158-4833-4f33-8d9a-d8af6067f779" />

### Configured a Key Pair - (Proceed without a Key Pair)

<img width="1500" height="892" alt="Screenshot 2026-01-26 143906" src="https://github.com/user-attachments/assets/d1224b82-7059-4412-a7da-8cc21e37906a" />

### Lab-VPC 

<img width="1500" height="341" alt="Screenshot 2026-01-26 144220" src="https://github.com/user-attachments/assets/f1bb492f-a651-455a-8ca0-ff73ed445615" />

### Storage: Default 8 GiB EBS root volume - remaines the same

<img width="1500" height="341" alt="Screenshot 2026-01-26 144336" src="https://github.com/user-attachments/assets/8659fe5d-22f9-4d8d-8270-31de37c2b3a9" />

EC2 Instance successfully lanuched

<img width="1500" height="341" alt="Screenshot 2026-01-26 144751" src="https://github.com/user-attachments/assets/c2900b6d-962e-4cdd-8d28-487b96e94906" />

Task 2: Monitoring the EC2 Instance
I completed status checks and verified system and instance reachability. With CloudWatch Metrics, I reviewed basic EC2 metrics like the CPU, network and system activity. I used the Get Instance Screenshot for the console-level visibility and troubleshootiing. 

### Monitoring 

<img width="1500" height="341" alt="Screenshot 2026-01-26 144948" src="https://github.com/user-attachments/assets/a9ac4215-6f17-41a7-9842-e0db76d944e7" />

### CloutWatch Metrics

<img width="1904" height="990" alt="Screenshot 2026-01-26 145449" src="https://github.com/user-attachments/assets/68b07572-62ff-4561-a68a-818d3d93c54a" />

### Get Insttance Sreenshot

<img width="1362" height="923" alt="Screenshot 2026-01-26 145728" src="https://github.com/user-attachments/assets/9711ec76-c4b9-42ab-b689-df858182b0b9" />

Task 3: Updating Security Group & Advanced Details 
My initial attempt to access the web server via the public IPv4 address failed because the HTTP(port 80) was not allowed. By updating the security group and allowing inbound rule to Type: HTTP; Port: 80 and the Source: Anywher(IPv4). My result successfully changed and I was able to access my web server which displayed "Hellow From Your Web Server!"

### Security Group configured to act like a firewall to control traffic for my instance

<img width="1500" height="341" alt="Screenshot 2026-01-26 144012" src="https://github.com/user-attachments/assets/823047ef-e1c7-45c5-8255-fa140c94b9dc" />

### Inserting Code in the User data text box to create a simple web page activates the server

<img width="1500" height="341" alt="Screenshot 2026-01-26 144704" src="https://github.com/user-attachments/assets/c11b1eea-6af2-49ca-ad83-b42977a42cb6" />

### Configuring the inbound rules of the web server security group

<img width="1500" height="341" alt="Screenshot 2026-01-26 160217" src="https://github.com/user-attachments/assets/533bdb5c-9f6e-45ee-83b3-b42468ecedb8" />

### Successfull Access to Webserver

Task 4: Resizing the EC2 Instance
It was required oof me to stop my instance and resize it from a t3.micro to a t3.small, effectively doubling its memory capacity. I also increased the root EBS volume from 8 GiB to 10 GiB, then restarted the instance. This showed me how an EC2 instance can be dynamically scaled to match changing workloads.

### From a t3.micro to a t3.small

<img width="1500" height="341" alt="Screenshot 2026-01-26 160748" src="https://github.com/user-attachments/assets/b0471373-54a7-4434-8f26-d1bfec1dc44d" />

### Volume Modified

<img width="1500" height="341" alt="Screenshot 2026-01-26 160958" src="https://github.com/user-attachments/assets/7933b3f5-1aac-433e-a65f-755ba1d92da5" />

Task 5: Test Termination Protection
The initial termination protection failed due to the termination protection  on the instance. This blocks you from terminating it. Once disables manually from the instance settings, I successfully terminated the instance. This task highlighted the importance of safeguards in managing production environments.

### Termination Failed

<img width="1500" height="341" alt="Screenshot 2026-01-26 161425" src="https://github.com/user-attachments/assets/464874d7-803f-4364-9ec3-7dddc7738e23" />

### Disbale Termination Protection

<img width="1500" height="341" alt="Screenshot 2026-01-26 161351" src="https://github.com/user-attachments/assets/7e6b3d08-702f-475a-886a-c16a8c12f113" />

### Instance terminated and deleted

<img width="1500" height="341" alt="Screenshot 2026-01-26 161752" src="https://github.com/user-attachments/assets/6593d0ec-6c31-41eb-ab22-ad56c9e9c9f0" />

### Key Concepts Covered
- How to securely launch and configure EC2 instances
- Importance of security groups as virtual firewalls
- Using User Data for automated configuration
- Monitoring EC2 health and performance
- Safely scaling compute and storage resources
- Preventing accidental deletions with termination protection

### My Learning Outcome
I successfully launched and accessed an EC2 instance, demonstrating foundational cloud compute knowledge, through this lab I also gained hands-on experience on the management console. I learned how to:

- Deploy and secure a web server using automation
- Use security groups to control network traffic effectively
- Monitor instance metrics, health and performance using CloudWatch
- Scale compute and storage resources dynamically
- Implement safeguards like termination protection to prevent accidental loss
- Overall, this experience deepened my understanding of cloud infrastructure management and gave me practical insight into - building resilient and scalable EC2 Instance applications.

Stacey L Munnik
Cloud & Software Engineering Learner
AWS EC2 Hands-on Lab (2026)

⭐ This project demonstrates practical experience with core AWS EC2 operations and cloud infrastructure management.
