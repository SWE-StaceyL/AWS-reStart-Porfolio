# Ahku Café – AWS Cloud Migration Solution

## Project Overview
Ahku Café is a fictional lifestyle café focused on calm coffee rituals and mindful living.  
This project demonstrates the migration of Ahku Café’s outdated, on-premises web presence to a **fully cloud-based solution using Amazon Web Services (AWS)**.

The goal of the migration is to improve availability, scalability, security, and cost efficiency while establishing a modern online presence.

---

## Problem Statement
Ahku Café previously relied on traditional, non-scalable infrastructure and had no reliable online platform to showcase its menu or engage customers.

Key challenges included:
- No online presence
- Limited customer reach
- High maintenance costs
- Lack of scalability
- Manual infrastructure management

---

## Solution Summary
The café was migrated to AWS using **Amazon S3 Static Website Hosting**, enabling a serverless, highly available, and cost-effective solution.

The new cloud architecture allows customers to:
- View the café menu
- Learn about the business
- Contact the café online

  ![cloud migration architecture](https://github.com/user-attachments/assets/dae4fbeb-246d-4d2f-bc23-34701f44c99f)


---

## AWS Services Used

### ☁️ Amazon S3 (Simple Storage Service)
- Hosts the static website (HTML, CSS, images)
- Provides high durability (99.999999999%)
- Automatically scales with traffic
- Low operational cost

 <img width="1912" height="952" alt="static website" src="https://github.com/user-attachments/assets/59e0c660-cdb5-4706-b072-f869cabb47af" />

 <img width="1912" height="952" alt="menu" src="https://github.com/user-attachments/assets/de82bad5-2da0-431e-8da4-6b8b5ca16d14" />
 
 <img width="1914" height="956" alt="Ccontactus" src="https://github.com/user-attachments/assets/2d3ec716-078b-4b99-9269-3e6a2fd41163" />


### 🔐 AWS IAM (Identity and Access Management)
- Secures access to AWS resources
- Enforces least-privilege access
- Protects the AWS account from unauthorized access

### 🌐 (Optional / Future Enhancements)
- **Amazon CloudFront** – Content Delivery Network for faster global access
- **Amazon Route 53** – Custom domain management
- **AWS Lambda** – Serverless backend for future order processing

---

## Architecture Overview
