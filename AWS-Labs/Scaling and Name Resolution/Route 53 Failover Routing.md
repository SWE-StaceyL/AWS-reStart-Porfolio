Amazon Route 53 Failover Routing Lab

### Project Structure
route53-failover-routing/

<img width="1121" height="708" alt="Screenshot 2026-03-17 102805" src="https://github.com/user-attachments/assets/f035235f-f056-4899-873a-ccde5ebedcca" />

### Overview

This lab demonstrates how to configure failover routing in Amazon Route 53 to improve application availability and resilience. Failover routing ensures that when a primary resource becomes unavailable, traffic is automatically redirected to a secondary (backup) resource.

The goal of this lab was to simulate a real-world disaster recovery scenario by configuring health checks and DNS failover between primary and secondary endpoints.

### Objectives

Understand how DNS failover works in AWS
Configure health checks to monitor endpoint availability
Implement failover routing policies
Test automatic failover behavior

### Services Used
Amazon Route 53
Amazon EC2

Route 53 Health Checks

### Step-by-Step Implementation
1. Launch EC2 Instances

Created two EC2 instances:
Primary instance (main application server)
Secondary instance (backup server)
Installed and configured a web server on both instances
Verified accessibility via public IP

Running Instances
<img width="1915" height="915" alt="Screenshot 2026-03-17 104140" src="https://github.com/user-attachments/assets/95102716-4fbe-4149-92b8-ee45187ad78b" />


Verified accessibility via public IP
<img width="1915" height="1030" alt="Screenshot 2026-03-17 104407" src="https://github.com/user-attachments/assets/bed5a334-dba1-4ab5-907e-16deb24defd9" />

<img width="1915" height="1011" alt="Screenshot 2026-03-17 104425" src="https://github.com/user-attachments/assets/ad504496-2592-4056-968e-b7e561d4f800" />


2. Configure Health Check

Navigated to Route 53 → Health Checks
Created a health check for the primary instance
Used HTTP protocol and configured endpoint
Confirmed status shows Healthy

Primary Health Check
<img width="1911" height="924" alt="Screenshot 2026-03-17 104813" src="https://github.com/user-attachments/assets/9d0a53a8-5837-4af5-8956-b43a9caffb0a" />

Health Check Confirmed Healthy
<img width="1894" height="846" alt="Screenshot 2026-03-17 111900" src="https://github.com/user-attachments/assets/579b4ece-85ac-4b28-b948-b807ad64ef4f" />


3. Create Hosted Zone

Created a hosted zone in Route 53
Configured domain for DNS management

Confirguration
<img width="1912" height="956" alt="Screenshot 2026-03-17 113033" src="https://github.com/user-attachments/assets/e5971630-ff1c-41c8-99c6-e56f0aca8f0f" />

4. Create Failover Routing Records
Primary Record
Routing policy: Failover (Primary)

Linked to health check
<img width="1903" height="943" alt="Screenshot 2026-03-17 113718" src="https://github.com/user-attachments/assets/1587ce98-2af6-402c-a087-2aacf171077f" />


Routing policy: Failover (Secondary)
<img width="1914" height="1000" alt="Screenshot 2026-03-17 113903" src="https://github.com/user-attachments/assets/76f04f4f-44c1-4a75-84ae-ed66acd63ce3" />
Accessed domain → routed to primary instance
Simulated failure (stopped server/instance)
Observed automatic redirection to secondary

<img width="1899" height="976" alt="Screenshot 2026-03-17 114908" src="https://github.com/user-attachments/assets/a3f199cd-10ec-43ad-af5c-6b058afa99b6" />

### Results

Successfully implemented DNS failover
Verified automatic redirection to backup system
Improved application availability

<img width="1915" height="920" alt="Screenshot 2026-03-17 114614" src="https://github.com/user-attachments/assets/990cca3c-5ae5-4e05-83c4-ae9f9836194a" />


 ### Key Learnings

Failover routing ensures high availability
Health checks monitor system reliability
DNS failover is a simple disaster recovery solution
TTL can introduce slight delay in failover
Critical for real-world cloud architectures


EC2 Instances
Health Check Configuration
Hosted Zone Setup
Primary Record Configuration
Secondary Record Configuration
Failover Test Result
│
# Real-World Use Case

Organizations use Route 53 failover routing to:
Maintain uptime during outages
Automatically switch to backup infrastructure
Improve user experience and reliability

### Conclusion

This lab provided practical experience in building a highly available and fault-tolerant system using AWS. It reinforced the importance of DNS-based failover in real-world cloud environments and is a key skill for cloud engineering roles.
