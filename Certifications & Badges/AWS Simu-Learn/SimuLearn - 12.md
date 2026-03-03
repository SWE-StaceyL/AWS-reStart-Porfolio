## AWS SimuLearn 12. AWS Connecting VPCs Lab

In this lab, I implemented secure connectivity between two separate Virtual Private Clouds (VPCs) within the Amazon Web Services (AWS) environment using VPC Peering. The primary objective was to enable private communication between resources deployed in different VPCs without traversing the public internet. To achieve this, I first verified that both VPCs had non-overlapping CIDR blocks and appropriate subnets configured. I then created a VPC Peering connection, accepted the peering request, and updated the route tables in each VPC to allow traffic to flow between the respective CIDR ranges. Additionally, I configured and validated the security group and Network ACL rules to permit inbound and outbound traffic between instances in both VPCs. After deployment, I tested connectivity using private IP addresses to confirm successful communication across the peered VPCs. This lab strengthened my understanding of AWS networking fundamentals, route table management, security controls, and hybrid network design principles, which are critical skills in Cloud Engineering and cloud infrastructure architecture.

<img width="1200" height="664" alt="ConnectingVPCs" src="https://github.com/user-attachments/assets/e93d66ce-9cce-4674-93a4-2d74f6cb4739" />


<img width="1200" height="664" alt="Screenshot 2026-03-03 125026" src="https://github.com/user-attachments/assets/47a1c197-e2c8-4d43-866f-c3d79065d595" />
