#  Python Prime Numbers Script — AWS EC2 Lab

##  Overview

This project was completed as part of an **AWS Challenge Lab: Python Scripting Exercise**. The goal was to connect to a remote Linux EC2 instance via SSH and write a Python 3 script that identifies and stores all prime numbers between 1 and 250.


##  Objectives

- [x] Connect to an AWS EC2 Linux instance via SSH
- [x] Write a Python 3 script to find all prime numbers between 1 and 250
- [x] Display the results in the terminal
- [x] Save the results to an output file (`results.txt`)
- [x] Verify the output is correct


##  Technologies Used

| Tool | Purpose |
|------|---------|
| **Python 3** | Core scripting language |
| **AWS EC2** | Remote Linux server environment |
| **SSH / PuTTY** | Secure remote connection |
| **Nano** | Terminal-based text editor |
| **Bash / Linux CLI** | File management and script execution |


## Project Structure

```
/home/ec2-user/
├── prime_numbers.py   # Main Python script
└── results.txt        # Output file containing all prime numbers
```

##  The Script

```python
def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True

primes = [n for n in range(1, 251) if is_prime(n)]

# Display to terminal
print("Prime numbers between 1 and 250:")
print(primes)

# Save to results.txt
with open("results.txt", "w") as f:
    f.write("Prime numbers between 1 and 250:\n")
    for p in primes:
        f.write(str(p) + "\n")

print("\nResults saved to results.txt")
```


##  How to Run

**1. Clone or copy the script to your machine:**
```bash
nano prime_numbers.py
# Paste the script content, then save with Ctrl+O and exit with Ctrl+X
```

**2. Run the script using Python 3:**
```bash
python3 prime_numbers.py
```

**3. View the output file:**
```bash
cat results.txt
```

<img width="1120" height="760" alt="Screenshot 2026-03-10 143547" src="https://github.com/user-attachments/assets/86144212-9a38-43a7-9bdb-ddf01a989af0" />


##  Expected Output

The script finds **53 prime numbers** between 1 and 250:

```
2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47,
53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107,
109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167,
173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229,
233, 239, 241
```

<img width="1120" height="760" alt="Screenshot 2026-03-10 144543" src="https://github.com/user-attachments/assets/afa068e8-0563-4f0c-88f1-08137f2858be" />


##  How It Works

The `is_prime(n)` function uses **trial division** with a square root optimization:

- Numbers less than 2 are **not prime**
- For each candidate number `n`, it checks for divisors from `2` up to `√n`
- If any divisor is found, the number is **not prime**
- If no divisors are found, the number **is prime**

> **Why √n?** If `n` has a factor larger than its square root, the corresponding paired factor must be smaller than the square root — so we only need to check up to `√n`.


##  Lab Environment

| Detail | Value |
|--------|-------|
| **Cloud Provider** | Amazon Web Services (AWS) |
| **Instance Type** | EC2 Linux Host |
| **OS** | Amazon Linux (EC2) |
| **Connection Method** | SSH (PEM key / PuTTY PPK) |
| **Lab Duration** | ~40 minutes |


##  License

This project was completed for educational purposes as part of AWS Training and Certification coursework.


*© 2022 Amazon Web Services, Inc. — Lab content referenced for educational use.*

Author: Stacey Munnik  
Program: Cloud & Software Engineering  
Lab: DevOps Automation Concepts

