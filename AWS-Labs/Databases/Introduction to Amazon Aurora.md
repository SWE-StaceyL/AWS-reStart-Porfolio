# Introduction to Amazon Aurora (AWS Lab)

### Overview

This lab introduces Amazon Aurora, a fully managed relational database engine available through Amazon Aurora and managed via Amazon RDS.

In this hands-on lab, I:

- Created an Aurora MySQL-compatible database cluster
- Connected to a pre-configured Amazon EC2 Linux instance
- Installed the MariaDB client
- Connected to Aurora using a cluster endpoint
- Created tables and inserted/query data using SQL

This lab demonstrates how AWS-managed databases integrate with compute resources in a secure VPC environment.

### Learning Objectives

- By completing this lab, I was able to:
- Provision an Aurora DB cluster
- Configure networking and security groups
- Connect to an EC2 Linux instance using Session Manager
- Install and use the MariaDB client
- Connect to Aurora using a cluster endpoint
- Create and manage relational tables
- Execute SQL queries

### Architecture Overview

- This lab environment consists of:
- Aurora MySQL-Compatible DB Cluster
- EC2 Linux Instance (Command Host)
- VPC with Private Subnets
- Custom DB Security Group
- The EC2 instance connects privately to the Aurora cluster using the Writer (Cluster) Endpoint inside the VPC.

### Lab Implementation Steps
1️. Create an Aurora Database

- Service Used: Amazon RDS
- Configuration Highlights:
- Engine Type: Aurora (MySQL Compatible)
- Engine Version: 8.0 (default)
- Template: Dev/Test
- DB Cluster Identifier: aurora
- Master Username: admin
- DB Instance Class: db.t3.medium
- Multi-AZ: Disabled (Lab environment)
- Initial Database Name: world
- Public Access: No
- VPC: LabVPC
- Security Group: DBSecurityGroup
- Encryption: Disabled (Lab setup)
- Result: Successfully provisioned an Aurora DB cluster.

2️. Connect to EC2 Linux Instance

- Service Used: Amazon EC2
- Steps:
- Navigated to EC2 → Instances
- Selected Command Host
- Connected via Session Manager
- Opened browser-based terminal
- 3️3. Install MariaDB Client
- sudo yum install mariadb -y

The MariaDB client enables connection to MySQL-compatible database engines such as Aurora.

4️. Connect to Aurora DB Cluster

- Retrieved the Cluster (Writer) Endpoint from RDS.
- Connection Command:
- mysql -u admin --password='admin123' -h <aurora-cluster-endpoint>

Successful connection returned:

Welcome to the MariaDB monitor.
MySQL [(none)]>

🗄️ Database Operations
📋 List Databases
SHOW DATABASES;

🔄 Switch to Database
USE world;

### Create Table

- Created a country table with fields including:
- Code (Primary Key)
- Name
- Continent
- Region
- SurfaceArea
- Population
- GNP
- GovernmentForm

### Insert Records

- Inserted multiple country records including:
- Australia
- Thailand
- Ireland
- Costa Rica
- Gabon

### Query Data
SELECT * 
FROM country 
WHERE GNP > 35000 
AND Population > 10000000;

Result:
Australia
Thailand
This confirmed successful table creation, data insertion, and querying.

### Key Concepts Learned
- Cluster Endpoint vs Reader Endpoint

- Cluster Endpoint
- Used for read/write operations
- Automatically fails over if primary instance changes
- Reader Endpoint
- Used for read-only queries
- Load balances across Aurora replicas
- VPC & Security
- Aurora deployed in private subnets
- EC2 connected internally via VPC
- Security group restricted access
- Public access disabled for security

### Skills Demonstrated

- AWS Console navigation
- RDS provisioning
- EC2 Session Manager usage
- Linux package management (yum)
- MySQL/MariaDB CLI usage
- SQL table creation and querying
- Understanding of database endpoints
- Cloud networking fundamentals

### Reflection

- This lab reinforced the importance of:
- Managed database services in cloud environments
- Secure connectivity within a VPC
- Understanding database endpoints
- Practical SQL database management
- Separation of compute and database layers

It demonstrated how Amazon Aurora provides high performance, scalability, and reliability while reducing administrative overhead compared to self-managed databases.

### Conclusion

Through this lab, I successfully:

✔ Created an Amazon Aurora DB cluster
✔ Connected to an EC2 Linux instance
✔ Configured secure database access
✔ Installed and used MariaDB client
✔ Created and queried relational tables

This lab strengthened my understanding of AWS database services and how compute resources securely interact with managed databases in a cloud-native architecture.
