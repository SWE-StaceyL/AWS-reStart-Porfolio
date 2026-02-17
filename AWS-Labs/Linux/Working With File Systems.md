Working with Files – AWS EC2 Linux Lab

<img width="1889" height="1001" alt="Screenshot 2026-01-29 092719" src="https://github.com/user-attachments/assets/05f8f56e-e754-4544-ac53-a373dfbf29ea" />

### Lab Overview
This lab focused on working with files in a Linux environment hosted on an Amazon EC2 instance. The primary objective was to create a full directory backup using tar, log the backup details, and transfer the backup file to another folder.
This hands-on exercise strengthened Linux command-line skills, file management, and basic backup operations within a cloud environment.

### AWS Services Used
- Amazon Elastic Compute Cloud (Amazon EC2) – Used to provision and access a virtual Linux server.
- Amazon Linux – Operating system running on the EC2 instance.
- t3.micro instance type – 1 vCPU and 1 GiB RAM (default lab configuration).

### Lab Objectives
In this lab, I:
- Connected to an EC2 instance using SSH
- Created a compressed backup of a full directory structure using tar
- Logged backup details (date, time, filename) into a CSV file
- Moved the backup file to a different folder for accessibility

### Task 1: Connect to EC2 via SSH
- I connected to an Amazon Linux EC2 instance using:
- Windows → PuTTY with .ppk key
- macOS/Linux → Terminal with .pem key
- Key Commands (macOS/Linux)
- chmod 400 labsuser.pem
- ssh -i labsuser.pem ec2-user@<public-ip>
This provided secure remote access to the Linux server.

<img width="1889" height="1001" alt="Screenshot 2026-01-29 071336" src="https://github.com/user-attachments/assets/3fc64e2d-2b30-4867-8b85-ff55738483ad" />

<img width="663" height="158" alt="Screenshot 2026-01-29 082223" src="https://github.com/user-attachments/assets/04dfe849-0f79-45e0-a916-97cf8f5116aa" />

### Task 2: Create a Backup Using tar
Existing Folder Structure
/home/ec2-user/CompanyA
├── Employees/
│   └── Schedules.csv
├── Finance/
│   └── Salary.csv
├── HR/
│   ├── Assessments.csv
│   └── Managers.csv
├── IA/
├── Management/
│   ├── Promotions.csv
│   └── Sections.csv
└── SharedFolders/

<img width="1895" height="1009" alt="Screenshot 2026-01-29 092534" src="https://github.com/user-attachments/assets/fac45674-c6ca-4bbf-8c48-1d7b4d548365" />

<img width="1900" height="1019" alt="Screenshot 2026-01-29 092608" src="https://github.com/user-attachments/assets/29d0d613-eeb7-4ffc-9cb3-0fe20b46cb02" />

Backup Command Used
tar -csvpzf backup.CompanyA.tar.gz CompanyA

Explanation of Flags
Flag	Meaning
-c	Create archive
-s	Preserve structure
-v	Verbose output
-p	Preserve permissions
-z	Compress using gzip
-f	Specify filename
Verification
ls
Output confirmed:

CompanyA
backup.CompanyA.tar.gz

### Task 3: Log the Backup Details

- To maintain backup history, I created a log file:
- touch SharedFolders/backups.csv
- Then added backup information:

echo "25 Aug 2021, 16:59, backup.CompanyA.tar.gz" | sudo tee SharedFolders/backups.csv
Viewing Log File
cat SharedFolders/backups.csv


This step demonstrated:

Use of echo
Pipe (|) redirection
tee command to write output to both terminal and file

### Task 4: Move the Backup File
- To make the backup accessible to another department (IA folder):
mv ../backup.CompanyA.tar.gz IA/
Verification
ls . IA
Confirmed the backup file was successfully moved to:
CompanyA/IA/backup.CompanyA.tar.gz

### Key Skills Demonstrated

- SSH remote server access
- Linux file navigation (cd, ls, pwd)
- Archive creation with tar
- File logging using echo and tee
- File movement using mv
- Understanding Linux file permissions

### What I Learned
This lab helped reinforce:

- How to create full directory backups in Linux
- The importance of backup logging for operational tracking
- Practical Linux command-line proficiency
- Managing files within a cloud-hosted virtual server
- Basic DevOps-style operational workflows

### Additional AWS Resources

Amazon EC2 Instance Types

Amazon Machine Images (AMIs)

EC2 Status Checks

EC2 Service Quotas

Terminating EC2 Instances

### Lab Completion
This lab was successfully completed in approximately 30 minutes.
All objectives were achieved, including backup creation, logging, and file transfer.
