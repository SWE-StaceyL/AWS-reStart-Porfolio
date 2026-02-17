# Performing a Conditional Search (AWS Lab)

### Overview
This lab demonstrates how to perform conditional searches in a relational database using SQL SELECT statements combined with the WHERE clause and related operators.
You will connect to a pre-configured database environment hosted on AWS and execute queries against the world database, which contains:

city
country
countrylanguage

The focus of this lab is filtering and aggregating data from the country table.

### Learning Objectives

- By completing this lab, you will be able to:
- Use the WHERE clause to filter records
- Apply comparison operators (=, >, <, >=, <=)
- Use the BETWEEN operator for range conditions
- Use the LIKE operator with wildcard characters
- Apply aggregate functions such as SUM()
- Use the AS keyword to create column aliases
- Use functions like LOWER() in SELECT and WHERE clauses
- Perform case-sensitive comparisons

### Lab Architecture
<img width="899" height="603" alt="Screenshot 2026-02-17 124133" src="https://github.com/user-attachments/assets/90757345-51f8-47dc-ab69-5bd91448bd1c" />

The lab environment includes:

- A Command Host EC2 instance
- A MySQL database client installed on the instance
- A preconfigured relational database named world
- Access through the AWS Management Console
- The user connects to the Command Host via Session Manager, then connects to the MySQL database server to execute SQL queries.

### Lab Tasks

### Task 1: Connect to the Command Host

- Open the AWS Management Console.
- Navigate to EC2 → Instances.
- Select the instance labeled Command Host.
- Choose Connect → Session Manager → Connect.
- Configure the terminal:

sudo su
cd /home/ec2-user/

Connect to the MySQL database:
- mysql -u root --password='re:St@rt!9'

<img width="950" height="988" alt="Screenshot 2026-02-16 154905" src="https://github.com/user-attachments/assets/7b4f237f-2359-43ae-a4b9-60f3a4bb3c73" />


### Task 2: Query the world Database
- Step 1: Verify Available Databases
  SHOW DATABASES;
- Ensure the world database is listed.
- Step 2: View Table Data
- SELECT * FROM world.country;

This retrieves all records from the country table.

### Conditional Searches
-  Using the WHERE Clause with Comparison Operators
- Filter countries with populations between 50 million and 100 million:

SELECT Name, Capital, Region, SurfaceArea, Population
FROM world.country
WHERE Population >= 50000000
AND Population <= 100000000;

### Using the BETWEEN Operator
- The BETWEEN operator simplifies range queries and is inclusive:

SELECT Name, Capital, Region, SurfaceArea, Population
FROM world.country
WHERE Population BETWEEN 50000000 AND 100000000;

### Using the LIKE Operator with Wildcards
- Find all countries in Europe and calculate total population:

SELECT SUM(Population)
FROM world.country
WHERE Region LIKE "%Europe%";

% is a wildcard representing any number of characters.

### Using Column Aliases (AS)
- Improve readability by renaming output columns:

SELECT SUM(Population) AS "Europe Population Total"
FROM world.country
WHERE Region LIKE "%Europe%";

### Case-Sensitive Search Using LOWER()
- Some databases enforce case sensitivity. Normalize values for comparison:

SELECT Name, Capital, Region, SurfaceArea, Population
FROM world.country
WHERE LOWER(Region) LIKE "%central%";

<img width="949" height="945" alt="Screenshot 2026-02-16 154818" src="https://github.com/user-attachments/assets/5c4d88d9-99ed-438e-94c6-2de284005af3" />


### Challenge Exercise
Objective
Return:
The sum of surface area
The sum of population
For countries in North America
Solution
SELECT 
    SUM(SurfaceArea) AS "N. America Surface Area",
    SUM(Population) AS "N. America Population"
FROM world.country
WHERE Region = "North America";

<img width="955" height="986" alt="Screenshot 2026-02-16 154725" src="https://github.com/user-attachments/assets/7780f748-6d3b-4fb8-9bc8-c62e151f64b3" />


### SQL Concepts Covered
Concept	Description
WHERE	Filters records based on conditions
AND	Combines multiple conditions
BETWEEN	Filters results within a range
LIKE	Searches for patterns using wildcards
SUM()	Aggregates numeric values
AS	Creates readable column aliases
LOWER()	Converts text to lowercase for comparison

### Lab Completion
Return to the lab interface.
Choose End Lab.
Confirm termination.
A success message will confirm the lab has ended.

### Data Source & References
Sample data provided by Statistics Finland (Creative Commons Attribution 4.0 International License).
MariaDB Documentation:

- SELECT
- WHERE
- BETWEEN
- LIKE
- SUM
- LOWER

### Key Takeaways

- The WHERE clause is essential for targeted data retrieval.
- BETWEEN improves readability for range conditions.
- LIKE enables flexible string matching.
- Aggregate functions like SUM() provide meaningful insights.
- Aliases (AS) improve query output clarity.
- Functions such as LOWER() help manage case sensitivity issues.

### Skills Demonstrated
- SQL Query Writing
- Database Filtering Techniques
- Data Aggregation
- AWS EC2 Session Manager Usage
- MySQL Command Line Interaction

### Conclusion
This lab provided practical, hands-on experience in performing conditional searches within a relational database environment hosted on AWS. By working directly with the world database, you strengthened your ability to retrieve, filter, and aggregate data using structured SQL queries.
