🐧 Introduction to the Linux Command Line
Amazon Linux AMI – AWS EC2 Lab

<img width="1904" height="979" alt="Screenshot 2026-01-28 084858" src="https://github.com/user-attachments/assets/869b966e-55e6-4ce5-85bb-c7cfe2946a81" />

### Overview

This project documents my hands-on lab experience working with the Linux Command Line using an Amazon Linux Amazon Machine Image (AMI) on an EC2 instance.

### The lab focused on:

- Connecting securely to a Linux server using SSH
- Understanding the Linux manual (man) pages
- Navigating and interpreting command documentation
- Exploring core Linux command-line functionality
- This lab was completed within a controlled AWS lab environment.

### AWS Services Used

The following AWS components were used:

- Amazon EC2 – Virtual server hosting the Linux instance
- Amazon Virtual Private Cloud – Network environment for the instance
- Amazon Linux AMI – Linux operating system image
- t3.micro instance type – 1 vCPU, 1 GiB memory

### Lab Objectives

- After completing this lab, I am able to:
- Use SSH to access a remote Linux server
- Understand and use the man command
- Navigate Linux manual pages
- Identify and interpret man page headers

### Understand Linux documentation structure

### Task 1: Connect to an Amazon Linux EC2 Instance via SSH
- What is SSH?
SSH (Secure Shell) allows secure remote access to a Linux server over the internet.

- Windows Users (PuTTY Method)
Download .ppk key file
Install and open PuTTY
Configure:
Host Name: ec2-user@<public-ip>
Authentication: Load .ppk file

<img width="1739" height="884" alt="Screenshot 2026-01-28 083041" src="https://github.com/user-attachments/assets/8503d61e-53ca-4c71-a63b-571099353e5f" />

<img width="1902" height="992" alt="Screenshot 2026-01-28 083307" src="https://github.com/user-attachments/assets/ba256915-cee6-4667-b837-b4958ce3e2d2" />

### Connect to the instance

- Connect to EC2 instance:
ssh -i labsuser.pem ec2-user@<public-ip>

- Type yes when prompted to trust the connection

### Key Learning

- SSH key authentication removes the need for passwords
- File permissions (chmod 400) are required for secure key usage
- Remote servers are managed entirely through the CLI

 <img width="1916" height="979" alt="Screenshot 2026-01-28 084948" src="https://github.com/user-attachments/assets/f501de61-a111-439c-ac9d-a530aa12820d" />

<img width="1855" height="932" alt="Screenshot 2026-01-28 085353" src="https://github.com/user-attachments/assets/7f0205bf-1465-4aad-ae84-fdf51e68404e" />

<img width="1894" height="1018" alt="Screenshot 2026-01-28 085718" src="https://github.com/user-attachments/assets/bc15440e-6324-48df-a777-cd9b5e571e4a" />

### Task 2: Exploring Linux Manual Pages (man Pages)

The Linux manual system provides built-in documentation for commands.
Open the man Page for the man Command
- man man
Important man Page Headers
4

When viewing a man page, I identified these key sections:

Header	Purpose
NAME	Command name and short description
SYNOPSIS	Command usage format
DESCRIPTION	Detailed explanation
OPTIONS	Available flags and switches
EXAMPLES	Practical usage examples
FILES	Related files
SEE ALSO	Related commands
🧭 Navigating man Pages

↑ ↓ Arrow keys – Scroll

/keyword – Search inside page

q – Quit

### Key Learning from man Pages

- Every Linux command has built-in documentation
- Section numbers identify command categories
- The DESCRIPTION section explains behavior in detail
- Understanding SYNOPSIS improves command accuracy

### Underlying Infrastructure

- This lab environment included:
- Public Subnet
- Amazon VPC
- EC2 Command Host
- Restricted AWS service permissions

Instance used:

- Instance Type: t3.micro
- vCPU: 1
- Memory: 1 GiB

### Security & Best Practices Learned

- Use key-based authentication instead of passwords
- Restrict private key permissions (chmod 400)
- Only access required AWS services
- Terminate instances after use
- Never share private keys

### Skills Gained

✔ Remote server access using SSH
✔ Linux command-line navigation
✔ Understanding built-in documentation
✔ Interpreting command structures
✔ Working inside AWS cloud infrastructure

### Outcome

This lab strengthened my foundational Linux skills within a real cloud environment.
It reinforced how Linux powers cloud infrastructure and how administrators manage servers entirely through the command line.

### These skills directly support:

- Cloud Engineering
- DevOps
- Backend Development

### Author

Stacey Munnik
Software Engineering & Cloud Engineering Student
Focus: Linux • AWS • Cloud Infrastructure • Security
