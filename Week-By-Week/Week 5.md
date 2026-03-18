

### Week 5 – AWS Networking & Content Delivery

####  Topics & Services Covered
- Subnets, Gateways & Route Tables
- Configuring and Deploying VPCs with Multiple Subnets
- AWS Client VPN — configure and deploy
- Amazon Direct Connect — dedicated network connection to AWS
- Amazon CloudFront — CDN and edge content delivery
- Elastic Network Interfaces (ENIs) — instance isolation
- AWS Global Accelerator — performance optimisation across regions
- AWS IPv6 Fundamentals & VPC Connectivity
- Application Load Balancer (ALB) & Network Load Balancer (NLB)
- Amazon API Gateway
- AWS Networking Cheat Sheets

####  Labs & Hands-On Projects
- Designed and configured a multi-subnet VPC architecture
- Set up route tables, internet gateways, and NAT gateways
- Explored ALB vs NLB routing behaviour and use cases
- Reviewed CloudFront distributions and caching configurations

####  Challenges & How I Solved Them
- **Challenge:** Understanding the difference between ALB and NLB and when to use each.
- **Solution:** ALB operates at Layer 7 (HTTP/HTTPS, path-based routing); NLB operates at Layer 4 (TCP/UDP, ultra-low latency). Use ALB for web applications, NLB for high-performance or low-latency network traffic.

####  Key Takeaways & Reflections
> Networking is the backbone of every AWS architecture. A well-designed VPC — with correct subnet segmentation, routing, and security controls — is foundational to both security and performance. CloudFront and Global Accelerator demonstrated how AWS pushes content and compute closer to end users around the world.


