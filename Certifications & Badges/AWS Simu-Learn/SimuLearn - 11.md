## AWS SimuLearn 11. Highly Avaiable Web Applications 

The objective of this lab was to design and validate a highly available web application architecture in AWS by ensuring fault tolerance, scalability, and distribution across multiple Availability Zones. To achieve this, I configured an Application Load Balancer to distribute incoming traffic evenly across EC2 instances running in separate Availability Zones, eliminating any single point of failure. I updated the existing Auto Scaling group to include a third Availability Zone and increased the desired capacity to ensure a minimum of three EC2 instances were running concurrently. This configuration allows the environment to automatically replace unhealthy instances and scale dynamically based on demand, ensuring continuous availability and resilience. Through these steps, I successfully implemented a multi-AZ, load-balanced, and auto-scaled architecture that meets AWS high availability best practices and passed the solution validation requirements.

<img width="1200" height="683" alt="HighlyAvailableWebApplications" src="https://github.com/user-attachments/assets/eceb115c-34e5-468c-95fd-248a8599f1f0" />

<img width="1062" height="822" alt="Screenshot 2026-03-03 112814" src="https://github.com/user-attachments/assets/9da048f7-cb3a-4417-85db-50b212bea0c5" />
