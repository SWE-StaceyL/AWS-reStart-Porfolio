### AWS re/Start Learning Journey

### Week 3 – AWS Storage Services

####  Topics & Services Covered
- AWS Storage Services Overview
- Amazon S3 — object storage essentials and storage classes
- Amazon EBS — block storage for EC2
- Amazon EFS — elastic file system
- Amazon FSx for Windows File Server
- Amazon FSx for Lustre — high-performance file system
- AWS Hybrid Storage Services
- AWS Edge Storage, Data Transfer & File Transfer Services
- AWS Storage Data Protection Services

####  Labs & Hands-On Projects
- Created and configured S3 buckets with access policies
- Explored EBS volume types and attachment to EC2 instances
- Reviewed EFS for shared file system access across multiple instances
- Studied hybrid storage options including AWS Storage Gateway

####  Challenges & How I Solved Them
- **Challenge:** Selecting the right S3 storage class for different data access patterns.
- **Solution:** Used a decision matrix mapping access frequency to cost: S3 Standard → Infrequent Access → Glacier Instant Retrieval → Glacier Deep Archive.

####  Key Takeaways & Reflections
> Storage is not one-size-fits-all in AWS. The choice between object (S3), block (EBS), and file (EFS/FSx) storage depends entirely on how the data is accessed and which workloads consume it. Understanding storage tiers and data lifecycle policies is essential for cost optimisation.

