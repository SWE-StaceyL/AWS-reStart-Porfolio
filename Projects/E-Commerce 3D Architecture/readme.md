# E-Commerce 3D Project Architecture

### Project Overview

This project demonstrates the design and implementation of a scalable, cloud-based 3D e-commerce architecture built using AWS services and modern web technologies.
The goal of this architecture is to support an online store that delivers an immersive 3D shopping experience, while ensuring:

- High availability
- Fault tolerance
- Scalability
- Security
- Performance optimization
- Cost efficiency

This project focuses heavily on cloud architecture design principles, infrastructure planning, and service integration.

### Architecture Overview

The architecture follows AWS Well-Architected Framework best practices and is designed using a multi-tier architecture pattern.

🔹 High-Level Components

- Frontend Layer – 3D Web Application 
- Application Layer – Backend APIs
- Database Layer – Managed relational database

🌐 Architecture Diagram

<img width="2228" height="3485" alt="aws_ecommerce_architecture_diagram" src="https://github.com/user-attachments/assets/9d477c36-bbde-4c63-893f-2033cf5e7a5d" />

### AWS Services Used
- Compute
Amazon EC2 (Application Servers)
Auto Scaling Group
Application Load Balancer (ALB)

- Database
Amazon RDS (MySQL)

- Storage
Amazon S3 (Static assets, 3D models, images)

-  Security
IAM Roles & Policies
Security Groups
NACLs
HTTPS via SSL/TLS

- Networking
Amazon VPC
Public Subnets (Load Balancer)
Private Subnets (EC2 & RDS)
Internet Gateway
NAT Gateway

-  Monitoring & Logging
Amazon CloudWatch
AWS CloudTrail

- 3D E-Commerce Flow

- User accesses the web application via HTTPS.
- Traffic is routed through the Application Load Balancer.
- Requests are forwarded to EC2 instances in an Auto Scaling Group.
- Backend APIs process requests.
- Product data is retrieved from Amazon RDS.
- 3D assets and images are served from Amazon S3.
- CloudWatch monitors system performance and logs activity.

### Security Considerations

- Database deployed in private subnet
- EC2 instances restricted via Security Groups
- Least-privilege IAM roles
- Encrypted data in transit (HTTPS)
- S3 bucket policies enforced
- Monitoring with CloudTrail for auditing

### Scalability & High Availability

- Auto Scaling ensures elasticity based on traffic demand
- Multi-AZ deployment for RDS
- Load balancing distributes traffic evenly
- Stateless application design
- Decoupled storage layer (S3)

### Cost Optimization

- On-demand instances for development
- Auto Scaling reduces idle compute costs
- S3 lifecycle policies for storage optimization
- Monitoring usage via AWS Cost Explorer

### What I Learned

- Through this project, I gained hands-on experience with:
- Designing multi-tier cloud architectures
- Implementing secure networking with VPC
- Deploying managed database services
- Integrating object storage for media-heavy applications
- Applying AWS Well-Architected Framework principles
- Planning for scalability and fault tolerance
- Designing architecture for immersive 3D web applications

### Future Improvements

- Implement Amazon CloudFront CDN
- Introduce containerization with Docker & ECS
- Add CI/CD pipeline (GitHub Actions / AWS CodePipeline)
- Implement caching layer (Redis / ElastiCache)
- Enable WAF for enhanced security
- Integrate payment gateway (Stripe / PayPal)

### Project Status

✅ Architecture Designed
✅ Services Configured
✅ Secure Network Implemented
✅ High Availability Enabled

📎 Author
Stacey Munnik
Cloud & Software Engineering Enthusiast
AWS Learner | Backend Development | Cloud Architecture
