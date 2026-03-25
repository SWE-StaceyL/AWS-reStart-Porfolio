# AWS S3 Challenge Lab — Static Website Hosting & Bucket Configuration
---
## Project Overview

This project documents the end-to-end configuration of an Amazon S3 environment using the **AWS CLI**, completed as part of the Praesignis AWS re/Start Programme. The lab covers core S3 concepts including bucket creation, static website hosting, access policies, versioning, lifecycle management, and cross-region replication.

**Live Website:** `http://cloudlearners-stacey-2024.s3-website-us-west-2.amazonaws.com`

<img width="1905" height="1001" alt="Screenshot 2026-03-25 095738" src="https://github.com/user-attachments/assets/c14616c0-34e4-47fa-8bd7-2b60f0b6ef21" />

---

---

## Tasks Completed

| # | Task | Method | Status |
|---|------|--------|--------|
| 1 | Create S3 Bucket | AWS CLI | ✅ Done |
| 2 | Upload Objects (index.html, error.html) | AWS CLI | ✅ Done |
| 3 | Configure Static Website Hosting | AWS CLI | ✅ Done |
| 4 | Enable S3 Versioning | AWS CLI | ✅ Done |
| 5 | Configure Lifecycle Rules | AWS CLI | ✅ Done |
| 6 | Cross-Region Replication (CRR) | AWS CLI | ✅ Done |

---

## AWS Services Used

- **Amazon S3** — Object storage, static website hosting, versioning, lifecycle, replication
- **AWS IAM** — Replication role and permissions policy
- **Amazon EC2** — CLI Host (Amazon Linux 2) used to run all commands

---

## CLI Commands Used

<img width="1906" height="959" alt="Screenshot 2026-03-25 094531" src="https://github.com/user-attachments/assets/d24771cc-d8c5-4b52-a741-0d96011b9c0f" />

### Task 1 — Create S3 Bucket

```bash
BUCKET_NAME="cloudlearners-stacey-2024"
REGION="us-west-2"

aws s3api create-bucket \
  --bucket $BUCKET_NAME \
  --region $REGION \
  --create-bucket-configuration LocationConstraint=$REGION

aws s3 ls
```

>  **Note:** `us-east-1` is the only region that does NOT require `--create-bucket-configuration`. All other regions must include it.

---

### Task 2 — Upload Objects

```bash
# Create index.html
cat > index.html << 'EOF'
<!DOCTYPE html>
<html>
  <head><title>CloudLearners Inc.</title></head>
  <body>
    <h1>Welcome to CloudLearners Inc.!</h1>
    <p>Hosted on Amazon S3</p>
  </body>
</html>
EOF

# Create error.html
cat > error.html << 'EOF'
<!DOCTYPE html>
<html>
  <head><title>Error - CloudLearners Inc.</title></head>
  <body>
    <h1>404 - Page Not Found</h1>
    <p>Sorry, that page does not exist.</p>
  </body>
</html>
EOF

# Upload to S3
aws s3 cp index.html s3://$BUCKET_NAME/
aws s3 cp error.html s3://$BUCKET_NAME/

# Verify
aws s3 ls s3://$BUCKET_NAME/
```

---

### Task 3 — Static Website Hosting

```bash
# Step 1: Disable Block Public Access
aws s3api put-public-access-block \
  --bucket $BUCKET_NAME \
  --public-access-block-configuration \
  "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"

# Step 2: Apply Public Read Bucket Policy
cat > bucket-policy.json << 'EOF'
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::cloudlearners-stacey-2024/*"
    }
  ]
}
EOF

aws s3api put-bucket-policy \
  --bucket $BUCKET_NAME \
  --policy file://bucket-policy.json

# Step 3: Enable Static Website Hosting
aws s3 website s3://$BUCKET_NAME/ \
  --index-document index.html \
  --error-document error.html

# Step 4: Get website URL
echo "http://$BUCKET_NAME.s3-website-$REGION.amazonaws.com"
```

---

### Task 4 — Enable Versioning

```bash
# Enable versioning
aws s3api put-bucket-versioning \
  --bucket $BUCKET_NAME \
  --versioning-configuration Status=Enabled

# Verify
aws s3api get-bucket-versioning --bucket $BUCKET_NAME

# Test by re-uploading modified index.html, then list versions
aws s3api list-object-versions --bucket $BUCKET_NAME
```

---

### Task 5 — Lifecycle Rules

```bash
cat > lifecycle.json << 'EOF'
{
  "Rules": [
    {
      "ID": "MoveToStandardIA",
      "Status": "Enabled",
      "Filter": { "Prefix": "" },
      "Transitions": [
        {
          "Days": 30,
          "StorageClass": "STANDARD_IA"
        }
      ]
    }
  ]
}
EOF

aws s3api put-bucket-lifecycle-configuration \
  --bucket $BUCKET_NAME \
  --lifecycle-configuration file://lifecycle.json

# Verify
aws s3api get-bucket-lifecycle-configuration --bucket $BUCKET_NAME
```

---

### Task 6 — Cross-Region Replication (CRR)

```bash
REPLICA_BUCKET="cloudlearners-stacey-replica"
REPLICA_REGION="us-east-1"

# Create destination bucket
aws s3api create-bucket \
  --bucket $REPLICA_BUCKET \
  --region $REPLICA_REGION

# Enable versioning on destination (required for CRR)
aws s3api put-bucket-versioning \
  --bucket $REPLICA_BUCKET \
  --versioning-configuration Status=Enabled

# Create IAM trust policy
cat > s3-trust-policy.json << 'EOF'
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "s3.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}
EOF

aws iam create-role \
  --role-name S3ReplicationRole \
  --assume-role-policy-document file://s3-trust-policy.json

# Attach replication permissions
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

cat > replication-permissions.json << 'EOF'
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetReplicationConfiguration","s3:ListBucket"],
      "Resource": "arn:aws:s3:::cloudlearners-stacey-2024"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:GetObjectVersion","s3:GetObjectVersionAcl"],
      "Resource": "arn:aws:s3:::cloudlearners-stacey-2024/*"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:ReplicateObject","s3:ReplicateDelete"],
      "Resource": "arn:aws:s3:::cloudlearners-stacey-replica/*"
    }
  ]
}
EOF

aws iam put-role-policy \
  --role-name S3ReplicationRole \
  --policy-name S3ReplicationPolicy \
  --policy-document file://replication-permissions.json

# Apply replication configuration
cat > replication.json << EOF
{
  "Role": "arn:aws:iam::$ACCOUNT_ID:role/S3ReplicationRole",
  "Rules": [
    {
      "ID": "ReplicateAll",
      "Status": "Enabled",
      "Filter": { "Prefix": "" },
      "Destination": {
        "Bucket": "arn:aws:s3:::cloudlearners-stacey-replica"
      },
      "DeleteMarkerReplication": { "Status": "Enabled" }
    }
  ]
}
EOF

aws s3api put-bucket-replication \
  --bucket $BUCKET_NAME \
  --replication-configuration file://replication.json
```

<img width="1908" height="923" alt="Screenshot 2026-03-25 095235" src="https://github.com/user-attachments/assets/1b4c28e3-6231-4f07-9aa1-533ce5f89df8" />

<img width="1910" height="930" alt="Screenshot 2026-03-25 095439" src="https://github.com/user-attachments/assets/32db1280-e1a5-4e95-861f-3758247bbece" />

<img width="1904" height="924" alt="Screenshot 2026-03-25 095659" src="https://github.com/user-attachments/assets/21e5a9ab-c40e-4908-953c-c50b1a136005" />

---

## Troubleshooting & Lessons Learned

| Error | Cause | Fix |
|-------|-------|-----|
| `InvalidLocationConstraint` | Used `us-east-1` with `--create-bucket-configuration` | Remove the flag for `us-east-1` only |
| `REGION` variable mismatch | `aws configure` set to `us-west-2` but variable set to `us-east-1` | Align variable with actual configured region |
| `NoSuchBucketPolicy` / website 404 | Bucket policy heredoc used `EOF` without single quotes, so `$BUCKET_NAME` didn't expand correctly | Use `'EOF'` (single quotes) or hardcode bucket name in policy JSON |
| `NoSuchWebsiteConfiguration` | Website command ran before policy was applied | Apply bucket policy first, then enable website hosting |
| Bash syntax errors from JSON output | Accidentally pasted JSON response back into terminal | Only paste commands, not output |

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| **S3 Bucket** | Global namespace container for objects. Name must be unique across all AWS accounts. |
| **Static Website Hosting** | Serves HTML/CSS/JS directly from S3 via a public endpoint — no server required. |
| **Bucket Policy** | JSON-based resource policy controlling who can perform which actions on a bucket. |
| **Versioning** | Preserves every version of every object. Required for CRR. Protects against accidental deletes. |
| **S3 Standard-IA** | Lower-cost storage for infrequently accessed data. Minimum 30-day storage, per-retrieval fee. |
| **Lifecycle Rules** | Automate transitioning objects to cheaper storage classes or expiring them after set time. |
| **CRR** | Cross-Region Replication — automatically copies new objects to a bucket in another region. |

---
## Conclusion
This challenge lab provided hands-on experience configuring Amazon S3 entirely through the AWS CLI — moving beyond the console and working the way cloud engineers do in real-world environments.
Completing this lab reinforced several important cloud skills:

CLI proficiency — managing AWS resources without a GUI builds deeper understanding of how services work under the hood
Debugging under pressure — real errors like InvalidLocationConstraint, NoSuchBucketPolicy, and heredoc variable expansion issues required careful reading of error messages and methodical troubleshooting
Security thinking — understanding why Block Public Access exists, when to disable it, and how bucket policies control access is foundational to secure cloud architecture
Cost awareness — lifecycle rules and storage class transitions reflect the kind of cost optimisation thinking expected of a Solutions Architect
Resilience by design — cross-region replication demonstrates how AWS enables high availability and disaster recovery across geographic boundaries

This project is part of my ongoing portfolio built during the Praesignis AWS re/Start Programme as I work toward AWS Cloud Practitioner certification and a career as a Cloud Engineer or Solutions Architect.


## 📄 License

This project is for educational purposes as part of the Praesignis AWS re/Start Programme.
