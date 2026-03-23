# AWS Lambda Word Count — Challenge Lab

---

## Lab Overview

This challenge lab demonstrates how to build a serverless word-counting pipeline on AWS. A Python Lambda function is automatically triggered whenever a `.txt` file is uploaded to an S3 bucket. It reads the file, counts the words, and sends the result to the user via email using Amazon SNS.

---

## Objectives

By completing this lab, the following skills were demonstrated:

- Creating an AWS Lambda function using Python 3.12
- Configuring an Amazon S3 bucket to trigger a Lambda function on file upload
- Creating and subscribing to an Amazon SNS topic for email notifications
- Applying the correct IAM role (`LambdaAccessRole`) for cross-service permissions
- Testing and verifying the pipeline end-to-end using CloudWatch Logs

---

## Architecture

```
You (upload file)
       │
       ▼
┌─────────────┐     S3 trigger     ┌──────────────────┐     publish     ┌─────────────┐
│  S3 Bucket  │ ─────────────────► │ Lambda Function  │ ──────────────► │  SNS Topic  │
│  (.txt file)│                    │  (Python 3.12)   │                 │  (Email)    │
└─────────────┘                    └──────────────────┘                 └─────────────┘
                                           │
                                    LambdaAccessRole
                                       (IAM Role)
```

**Services Used:**

| Service | Purpose |
|---|---|
| Amazon S3 | Stores uploaded text files and triggers Lambda |
| AWS Lambda | Serverless function that reads the file and counts words |
| Amazon SNS | Sends the word count result via email notification |
| AWS IAM | `LambdaAccessRole` grants Lambda access to S3, SNS, and CloudWatch |
| Amazon CloudWatch | Captures Lambda execution logs for debugging |

---

### Step 1 — Create an SNS Topic and Subscribe

1. Navigate to **SNS** → **Topics** → **Create topic**
2. Type: **Standard** | Name: `WordCountTopic`
3. Click **Create topic** and copy the **Topic ARN**
4. Click **Create subscription** → Protocol: **Email** → enter your email address
5. Confirm the subscription by clicking the link sent to your inbox

> The subscription must show **Confirmed** status before testing, or emails will not be delivered.

<img width="1907" height="930" alt="Screenshot 2026-03-23 141243" src="https://github.com/user-attachments/assets/f4c60cf6-4fa5-4342-b907-42c3a3fe3d67" />

<img width="1060" height="422" alt="Screenshot 2026-03-23 152535" src="https://github.com/user-attachments/assets/e020d643-322b-4573-b189-44715430e372" />

---

### Step 2 — Create an S3 Bucket

1. Navigate to **S3** → **Create bucket**
2. Provide a globally unique name (e.g. `wordcount-yourname-2024`)
3. Select a region and keep it consistent across all services
4. Leave all other settings as default → **Create bucket**

<img width="1912" height="927" alt="Screenshot 2026-03-23 141911" src="https://github.com/user-attachments/assets/92d87cb7-4553-492e-b324-c2302a224233" />

---

### Step 3 — Create the Lambda Function

1. Navigate to **Lambda** → **Create function** → **Author from scratch**
2. Settings:
   - Function name: `WordCountFunction`
   - Runtime: **Python 3.12**
   - Architecture: `x86_64`
3. Under **Permissions** → **Change default execution role** → **Use an existing role** → select **LambdaAccessRole**
4. Click **Create function**

<img width="1882" height="917" alt="Screenshot 2026-03-23 142145" src="https://github.com/user-attachments/assets/24a02469-e468-4e4d-bc21-f6215dc47034" />

---

### Step 4 — Write the Lambda Code

In the **Code source** editor, replace all existing code with the following:

python
import json
import boto3
import urllib.parse

sns_client = boto3.client('sns')

# Replace with your SNS Topic ARN
SNS_TOPIC_ARN = 'arn:aws:sns:REGION:ACCOUNT_ID:WordCountTopic'

def lambda_handler(event, context):
    # Retrieve bucket name and file key from the S3 event
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = urllib.parse.unquote_plus(
        event['Records'][0]['s3']['object']['key']
    )

    # Read the uploaded file from S3
    s3_client = boto3.client('s3')
    response = s3_client.get_object(Bucket=bucket, Key=key)
    content = response['Body'].read().decode('utf-8')

    # Count the words
    word_count = len(content.split())

    # Format the message
    message = f"The word count in the {key} file is {word_count}."

    # Publish to SNS
    sns_client.publish(
        TopicArn=SNS_TOPIC_ARN,
        Subject='Word Count Result',
        Message=message
    )

    print(message)
    return {
        'statusCode': 200,
        'body': json.dumps(message)
    }


Click **Deploy** to save the function.

<img width="1897" height="953" alt="Screenshot 2026-03-23 142234" src="https://github.com/user-attachments/assets/db120e0e-f012-4406-9d35-e87ec21b6d6c" />


### Step 5 — Add the S3 Trigger

1. In the Lambda function page, click **+ Add trigger**
2. Select **S3**
3. Choose your bucket from Step 2
4. Event type: **PUT**
5. Suffix: `.txt`
6. Acknowledge the notice → click **Add**

<img width="1885" height="951" alt="Screenshot 2026-03-23 142534" src="https://github.com/user-attachments/assets/93099f6c-ba3c-4045-ab93-4bc85c823452" />
---

### Step 6 — Test the Pipeline

1. Create sample text files locally (e.g. `test1.txt`, `test2.txt`) with varying word counts
2. Upload each file to the S3 bucket via the console
3. Wait approximately 30 seconds, then check your inbox for an email like:

```
Subject: Word Count Result
Body: The word count in the test1.txt file is 42.
```

<img width="1897" height="959" alt="Screenshot 2026-03-23 143453" src="https://github.com/user-attachments/assets/efd45b3e-74e5-49ad-9cb8-189f5ed3feaa" />

---

### Step 7 — Verify with CloudWatch Logs (if needed)

If no email arrives:

1. Navigate to **CloudWatch** → **Log groups** → `/aws/lambda/WordCountFunction`
2. Open the latest log stream to view execution output and any error messages
3. Common issues to check:
   - Incorrect SNS Topic ARN in the function code
   - Email subscription not confirmed
   - S3 bucket and Lambda function in different AWS regions


## Key Concepts Learned

- **Serverless computing** — Lambda functions run without provisioning or managing servers, charging only for execution time
- **Event-driven architecture** — S3 triggers Lambda automatically on file upload, with no polling required
- **IAM least-privilege** — The `LambdaAccessRole` grants only the specific permissions Lambda needs (S3, SNS, CloudWatch)
- **Pub/Sub messaging** — SNS decouples the notification logic from the compute logic, making the system easier to extend
- **Observability** — CloudWatch Logs capture Lambda output, making debugging straightforward without SSH or server access

---

##  Conclusion

This lab brought together four core AWS services — Lambda, S3, SNS, and IAM — to build a fully serverless, event-driven pipeline with zero infrastructure to manage.
The experience reinforced how powerful the AWS serverless ecosystem is for automating tasks that would otherwise require dedicated servers, scheduled jobs, or manual intervention. By configuring an S3 event trigger, the function fires the moment a file lands in the bucket — making the pipeline both responsive and cost-efficient.
Working through this challenge also highlighted the importance of IAM role configuration, correct region consistency, and SNS subscription confirmation as foundational steps that must be verified before testing. Debugging via CloudWatch Logs proved essential when resolving issues during the testing phase.
This project forms part of the **CloudLearners Inc.** portfolio series built during the **AWS re/Start Programme**, and demonstrates practical, hands-on experience with serverless architecture on AWS.

---

*Built as part of the Praesignis AWS re/Start Programme | PTA, South Africa*
