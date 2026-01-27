AWS EC2 Hands-On Lab

### Overview
This lab demonstrates practical experience with Amazon EC2 (Elastic Compute Cloud), a core AWS service that provides scalable computing capacity in the cloud. During this lab, I worked through the process of launching, configuring, securing, monitoring, and scaling EC2 instances, gaining a clear understanding of how cloud infrastructure behaves in real-world scenarios.

By completing this lab, I not only deployed a working web server but also explored security, automation, and instance lifecycle management, which are critical for building reliable and resilient cloud applications.

### Lab Architecture
The lab involved deploying a web server on EC2 with proper security and scalability:

image

### Lab Work

1️⃣ Launch EC2 Instance
I began by launching an EC2 instance named Web Server using the Amazon Linux 2023 AMI. I selected a t3.micro instance type and configured termination protection to prevent accidental deletion. Using a User Data script, I automated the installation of an Apache web server and deployed a simple web page.

Screenshot Placeholder
image image image

2️⃣ Monitor the Instance
Once the instance was running, I explored monitoring options. I checked system and instance reachability via Status Checks, reviewed CloudWatch metrics for performance insights, and captured an instance screenshot to simulate console access.

Screenshot Placeholder
image image image

3️⃣ Configure Security and Access the Web Server
Initially, my instance was not accessible through a browser because the security group blocked HTTP traffic. I modified the security group to allow inbound traffic on port 80 and successfully accessed the web server, confirming that the automation script had deployed the webpage correctly:

Screenshot Placeholder
image image image

4️⃣ Resize the Instance
As part of understanding scalability, I stopped the instance and resized it from a t3.micro → t3.small, effectively doubling its memory. I also increased the root EBS volume from 8 GiB to 10 GiB, then restarted the instance. This step illustrated how EC2 instances can be dynamically scaled to match changing workloads.

Screenshot Placeholder
image image image image image image

5️⃣ Test Termination Protection
I tested termination protection by attempting to terminate the instance, which was initially blocked. After disabling the protection, I successfully terminated the instance. This task highlighted the importance of safeguards in managing production environments.

Screenshot Placeholder
image image image image image image image image

### Key Concepts Covered
- EC2 instance creation
- Instance types and sizing
- Key pairs and security groups
- Connecting to an EC2 instance

### My Learning Outcome
I successfully launched and accessed an EC2 instance, demonstrating foundational cloud compute knowledge, through this lab I also gained hands-on experience on the management console. I learned how to:

- Deploy and secure a web server using automation
- Use security groups to control network traffic effectively
- Monitor instance metrics, health and performance using CloudWatch
- Scale compute and storage resources dynamically
- Implement safeguards like termination protection to prevent accidental loss
- Overall, this experience deepened my understanding of cloud infrastructure management and gave me practical insight into building resilient and scalable EC2 Instance applications.
