
# Internet Protocols – Public and Private IP Addresses

### Overview

This lab simulates a real-world cloud support scenario at **Amazon Web Services (AWS)**. Acting as a cloud support engineer, I investigated a networking issue reported by a Fortune 500 customer involving two Amazon EC2 instances — one that could reach the internet and one that could not — despite both being in the same subnet with the same AWS resource configurations.


### Objectives

- Summarize and investigate the customer scenario
- Analyze the difference between **private** and **public** IP addresses
- Develop a solution to the customer's connectivity issue
- Summarize and present findings


### Customer Scenario

> **Ticket submitted by:** Jess (Cloud Admin) — Fortune 500 Company

The customer's environment:

- **VPC CIDR Range:** `10.0.0.0/16`
- **Two EC2 instances:** Instance A and Instance B
- Both instances are in the **same subnet** with the **same AWS resource configurations**
- **Problem:** Instance A **cannot** reach the internet; Instance B **can**
- **Secondary question:** Would using a public IP range such as `12.0.0.0/16` for a new VPC cause any issues?

**Architecture:** VPC → Internet Gateway → Public Subnet (Instance A & Instance B)


### Investigation & Steps Taken

### Task 1 – Replicate and Understand the Customer's Environment

1. Opened the **AWS Management Console** and navigated to the **EC2** service via the search bar or **Services → Compute → EC2**.
2. In the EC2 dashboard, navigated to **Instances** in the left navigation menu to view both running instances.
3. Selected **Instance A**, opened the **Networking** tab, and recorded the **Public** and **Private IPv4 addresses**.
4. Deselected Instance A, then repeated the process for **Instance B**.

**Key Observation:** Instance A had **no public IPv4 address assigned**, while Instance B had both a public and private IPv4 address. This was the root cause of the connectivity issue.

<img width="1918" height="984" alt="Screenshot 2026-02-23 132714" src="https://github.com/user-attachments/assets/5027e375-13a6-4610-9a79-ad38905d729b" />


### Task 2 – Analyze the Difference Between Public and Private IP Addresses

| Feature | Private IP Address | Public IP Address |
|---|---|---|
| Scope | Internal/local network only | Globally routable on the internet |
| Assignment | Automatically assigned within the VPC CIDR range | Assigned by AWS from a public pool |
| Internet Access | Cannot directly reach the internet | Can communicate over the internet |
| Example Range | `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` | Any IP outside the RFC 1918 private ranges |
| Cost | Free | May incur charges |

**Private IP ranges (RFC 1918):**
- `10.0.0.0 – 10.255.255.255`
- `172.16.0.0 – 172.31.255.255`
- `192.168.0.0 – 192.168.255.255`

<img width="1916" height="960" alt="Screenshot 2026-02-23 133930" src="https://github.com/user-attachments/assets/f0b72e74-3939-42a1-9c2c-236d3eb327ca" />

### Task 3 – Develop a Solution

#### Root Cause
Instance A lacked a **public IPv4 address**. Even though both instances were in the same public subnet with an internet gateway attached, Instance A could not communicate with the internet because it had no public IP to route traffic through the gateway.

#### Solution
Assign a **public IPv4 address** to Instance A. This can be done by:

- **Option 1:** Enabling **Auto-assign Public IP** when launching or modifying the instance's subnet settings.
- **Option 2:** Allocating and associating an **Elastic IP (EIP)** address to Instance A from the EC2 console:
  1. Go to **EC2 → Network & Security → Elastic IPs**
  2. Click **Allocate Elastic IP address**
  3. Select the new EIP and click **Actions → Associate Elastic IP address**
  4. Choose Instance A and associate
 
<img width="1888" height="917" alt="Screenshot 2026-02-23 134225" src="https://github.com/user-attachments/assets/835defce-3671-49f1-9984-e172df4d9582" />


#### Addressing the Secondary Question: Using `12.0.0.0/16` for a new VPC

Using a **public IP range** (like `12.0.0.0/16`) as the CIDR block for a VPC is **technically allowed by AWS** but is **strongly discouraged** for the following reasons:

- `12.0.0.0/16` is a **publicly routable IP range** owned by AT&T, not a private RFC 1918 range.
- Traffic destined for real `12.x.x.x` addresses on the internet would be **misrouted** within the VPC.
- Services, APIs, or external systems using addresses in the `12.0.0.0/16` range would become **unreachable** from within the VPC.
- This can cause unpredictable routing behavior and connectivity failures.

**Recommendation:** Always use RFC 1918 private IP ranges for VPC CIDR blocks (e.g., `10.0.0.0/16`).


### Findings Summary

| Issue | Root Cause | Solution |
|---|---|---|
| Instance A cannot reach the internet | No public IPv4 address assigned | Assign an Elastic IP or enable auto-assign public IP |
| Using `12.0.0.0/16` for a VPC | Public IP range — not private RFC 1918 | Use `10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16` instead |


### Key Concepts Learned

- The difference between **public** and **private** IP addresses and their roles in AWS networking
- How an **Internet Gateway** enables internet access but requires a public IP on the EC2 instance to function
- The importance of using **RFC 1918 private address ranges** for VPC CIDR blocks
- How to diagnose and resolve **EC2 internet connectivity issues** using the Networking tab in the AWS Console
- How to assign **Elastic IP addresses** to EC2 instances


### AWS Services Used

- **Amazon VPC** – Virtual Private Cloud for network isolation
- **Amazon EC2** – Elastic Compute Cloud instances
- **Internet Gateway** – Enables internet communication for the VPC
- **Elastic IP (EIP)** – Static public IPv4 address for AWS resources

