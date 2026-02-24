
## AWS Cloud Support Lab | VPC Networking & Security Groups |  Troubleshooting a Network Issue


###Overview

This lab simulates a real-world cloud support scenario at **Amazon Web Services (AWS)**. Acting as a cloud support engineer, I investigated a networking issue reported by a consulting company (Ana Contractor) who was unable to ping or access her **Apache HTTP server** via a browser, despite it being installed on an EC2 instance within her VPC.

### Objectives

- Analyze the customer scenario and replicate their environment
- Install and verify the Apache HTTP server (`httpd`) on an EC2 instance
- Investigate VPC configuration (subnets, route tables, internet gateway, security groups)
- Identify and resolve the root cause blocking web traffic
- Confirm the Apache server loads successfully in a browser


### Customer Scenario

> **Ticket submitted by:** Ana Contractor — Consulting Company

**The problem:** Ana created an Apache server via the command line but could not ping it or access it through the browser using the instance's public IP address.

**Architecture:** VPC → Internet Gateway → Public Subnet → EC2 Instance (Apache/httpd)


### Step-by-Step Walkthrough

### Accessing the AWS Management Console

1. Launched the lab environment and waited for **Lab status: ready**.
2. Opened the **AWS Management Console** from the lab panel.


### Task 1 – Connect to the EC2 Instance via SSH

3. Navigated to **EC2** using the search bar or **Services → Compute → EC2**.
4. In the left navigation menu, selected **Instances** to view the running instance.
5. Selected the instance and opened the **Details** tab to note the **Public IPv4 address**.
6. Closed the Details panel.
7. Opened a terminal and navigated to the directory containing the `labsuser.pem` key file:

```bash
cd ~/Downloads
```

8. Set the key file to read-only permissions to satisfy SSH requirements:

```bash
chmod 400 labsuser.pem
```

9. Connected to the EC2 instance via SSH (replacing `<public-ip>` with the instance's actual public IP):

```bash
ssh -i labsuser.pem ec2-user@<public-ip>
```

10. Typed `yes` when prompted to accept the SSH fingerprint. No password was required since key-pair authentication was used.

<img width="1902" height="920" alt="Screenshot 2026-02-23 171640" src="https://github.com/user-attachments/assets/aa87d029-f3ee-4df3-a7e8-847922add92a" />

<img width="1914" height="1030" alt="Screenshot 2026-02-23 172034" src="https://github.com/user-attachments/assets/c9a6ba1d-d1bf-478f-ac2d-7643a58acf50" />


### Task 2 – Install and Start the Apache HTTP Server (httpd)

The goal of this task was to install and verify `httpd` before investigating the VPC configuration.

11. Checked the current status of the `httpd` service:

```bash
sudo systemctl status httpd.service
```

> **Result:** The service was **inactive (dead)** — it was installed but not yet running.

12. Started the `httpd` service:

```bash
sudo systemctl start httpd.service
```

13. Verified the service was now running:

```bash
sudo systemctl status httpd.service
```

> **Result:** The service was now **active (running)**.

14. Attempted to access the Apache test page in a browser using the instance's public IP:

```
http://<PUBLIC IP OF INSTANCE>
```

> **Result:** The page **did not load** at this stage — confirming a network-level issue beyond just the service status.

<img width="1906" height="1035" alt="Screenshot 2026-02-23 172012" src="https://github.com/user-attachments/assets/d90a8466-b431-4e48-8bf1-858e26f8b692" />

<img width="1919" height="1034" alt="Screenshot 2026-02-23 171942" src="https://github.com/user-attachments/assets/17a3bd9f-a995-4a65-b1df-024d70111e2c" />


### Task 3 – Investigate the VPC Configuration

With the Apache server confirmed as running but still inaccessible, I moved on to systematically audit the VPC configuration in the AWS Console.

15. Navigated to **VPC** via **Services → Networking & Content Delivery → VPC**.
16. Used the left navigation pane to check each VPC component:

#### a. Subnets
- Verified that the route table was correctly associated with the intended public subnet.

#### b. Route Tables
- Confirmed the route table contained a route sending `0.0.0.0/0` traffic to the Internet Gateway, enabling outbound internet access.

#### c. Internet Gateway
- Confirmed an Internet Gateway existed and was **attached** to the VPC.

#### d. Security Groups & Network ACLs *(Root Cause Found Here)*
- Reviewed the **inbound rules** of the security group attached to the EC2 instance.
- **Discovered:** The security group was missing inbound rules for **HTTP (port 80)** and/or **HTTPS (port 443)**.
- Apache uses HTTP/S ports to serve web traffic — without these rules open, all browser requests were being blocked.

**Fix Applied:** Added the following inbound rules to the security group:

| Type  | Protocol | Port Range | Source    |
|-------|----------|------------|-----------|
| HTTP  | TCP      | 80         | 0.0.0.0/0 |
| HTTPS | TCP      | 443        | 0.0.0.0/0 |

17. Tested connectivity by pinging an external site from the instance to confirm the internet gateway and routing were working:

```bash
ping www.amazon.com
```

18. Navigated back to the browser and entered the instance's public IP:

```
http://<PUBLIC IP OF INSTANCE>
```

> **Result:** ✅ The **Apache HTTP Server test page** loaded successfully.

---

### ✅ Root Cause & Resolution

| Issue | Root Cause | Resolution |
|---|---|---|
| Apache server unreachable via browser | Security group missing inbound rules for HTTP (port 80) / HTTPS (port 443) | Added HTTP and HTTPS inbound rules to the EC2 security group |
| Cannot ping the instance | ICMP traffic blocked by security group | Add an inbound rule for **All ICMP - IPv4** from `0.0.0.0/0` |

<img width="1912" height="990" alt="Screenshot 2026-02-23 171744" src="https://github.com/user-attachments/assets/7241c923-4e43-4c93-bd66-944cf8f5569f" />


### Key Concepts Learned

- How to **SSH into an EC2 instance** using a `.pem` key pair
- How to manage Linux services using **`systemctl`** (start, status)
- How **AWS Security Groups** act as virtual firewalls and the importance of configuring correct inbound rules
- How to perform a **systematic VPC audit** — checking subnets, route tables, internet gateways, and security groups
- That Apache (`httpd`) requires **port 80 (HTTP)** to be open in the security group to serve web traffic
- The layered nature of AWS networking: even if a server is running, traffic can still be blocked at the security group or network ACL level


### AWS Services Used

| Service | Purpose |
|---|---|
| **Amazon EC2** | Hosted the Apache web server |
| **Amazon VPC** | Isolated network environment for the instance |
| **Internet Gateway** | Enabled inbound/outbound internet traffic |
| **Route Tables** | Directed traffic from the subnet to the internet gateway |
| **Security Groups** | Controlled inbound/outbound traffic at the instance level |
| **Network ACLs** | Subnet-level traffic filtering |

---

### Key Commands Used

```bash
# Navigate to key file directory
cd ~/Downloads

# Set correct permissions on PEM key
chmod 400 labsuser.pem

# SSH into EC2 instance
ssh -i labsuser.pem ec2-user@<public-ip>

# Check Apache service status
sudo systemctl status httpd.service

# Start Apache service
sudo systemctl start httpd.service

# Test internet connectivity from instance
ping www.amazon.com
```

---

