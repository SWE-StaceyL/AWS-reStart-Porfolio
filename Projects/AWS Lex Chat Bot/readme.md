### AWS VPC Quiz Chatbot

 An interactive AWS Networking knowledge quiz chatbot built for **CloudLearners Inc.** using Amazon Lex, AWS Lambda, and Amazon S3.

## Project Overview

This project delivers a fully functional conversational quiz chatbot that tests users on AWS VPC and networking concepts. Users interact with the bot through natural language, answer multiple-choice questions, and receive instant feedback with explanations. The bot covers **15 questions** across Beginner and Intermediate difficulty levels.

Built as part of the **Praesignis AWS re/Start Programme — Project 3**.

## Architecture

User Input
    │
    ▼
┌─────────────────┐
│   Amazon Lex    │  ← Receives input, identifies intent, manages conversation flow
│  (VPCQuizBot)   │
└────────┬────────┘
         │  Fulfillment trigger
         ▼
┌─────────────────┐
│  AWS Lambda     │  ← Evaluates answers, tracks score, fetches next question
│ (VPCQuizHandler)│
└────────┬────────┘
         │  boto3 SDK (S3 GetObject)
         ▼
┌─────────────────┐
│   Amazon S3     │  ← Stores quiz_questions.json (question bank)
│  (Quiz Bucket)  │
└─────────────────┘


| Service | Role | Analogy |
|---|---|---|
| **Amazon Lex** | Understands user input, manages intents and conversation flow | The Receptionist |
| **AWS Lambda** | Evaluates answers, tracks score, fetches questions | The Quiz Master |
| **Amazon S3** | Stores the JSON question bank | The Filing Cabinet |

## Features

- 🗣️ **Natural language interface** — recognises varied phrases like *"start quiz"*, *"quiz me"*, *"let's start"*
- ✅ **Instant answer feedback** — tells the user if they were correct and explains why
- 📊 **Score tracking** — persists score across all 15 questions using Lex session attributes
- 🔁 **Sequential question flow** — automatically advances to the next question after each answer
- 🎓 **Two difficulty tiers** — 7 Beginner questions and 8 Intermediate questions
- 🔒 **Secure by design** — S3 bucket is private; Lambda accesses it via IAM role (no public URLs)
- 📝 **CloudWatch logging** — Lambda execution logs available for debugging

## Quiz Topics Covered

| # | Topic | Difficulty |
|---|---|---|
| 1 | CIDR notation | Beginner |
| 2 | Internet Gateway | Beginner |
| 3 | Public vs Private Subnets | Beginner |
| 4 | EC2 internet access requirements | Beginner |
| 5 | NAT Gateway function | Beginner |
| 6 | Elastic Network Interfaces (ENI) | Beginner |
| 7 | Amazon Route 53 | Beginner |
| 8 | Security Groups vs NACLs | Intermediate |
| 9 | NACL rule processing order | Intermediate |
| 10 | VPC Peering limitations | Intermediate |
| 11 | AWS Transit Gateway | Intermediate |
| 12 | Direct Connect vs VPN | Intermediate |
| 13 | Application Load Balancer (Layer 7) | Intermediate |
| 14 | VPC Interface Endpoints & PrivateLink | Intermediate |
| 15 | AWS PrivateLink | Intermediate |

## AWS Services Used

- **Amazon Lex V2** — Conversational AI bot with two intents and a custom slot type
- **AWS Lambda** — Python 3.12 function for quiz logic and answer evaluation
- **Amazon S3** — Private bucket storing `quiz_questions.json`
- **AWS IAM** — Custom role (`LambdaLexQuizRole`) with scoped S3 and CloudWatch permissions
- **Amazon CloudWatch** — Automatic Lambda execution logging

## Project Structure

aws-lex-quiz-chatbot/
│
├── lambda/
│   └── lambda_function.py        # Quiz logic: answer checking, score tracking, question flow
│
├── quiz-data/
│   └── quiz_questions.json       # 15 VPC networking questions with answers and explanations
│
└── README.md

## Build & Deployment Steps

### Step 1 — S3 (Question Bank)
1. Create a private S3 bucket (e.g. `netquizquiz-bot-stacey`)
2. Upload `quiz_questions.json` to the bucket
3. Note the S3 URI for use in the Lambda function
   
<img width="1906" height="930" alt="Screenshot 2026-03-12 073931" src="https://github.com/user-attachments/assets/4b55cc03-4413-4371-9d9c-0d280eeec4bd" />

### Step 2 — IAM Role
1. Create an IAM role named `NetQuizRole`
2. Attach policies: `AmazonS3ReadOnlyAccess` + `AWSLambdaBasicExecutionRole`

IAM Role 
<img width="1907" height="925" alt="Screenshot 2026-03-12 081018" src="https://github.com/user-attachments/assets/e30d6d72-a957-430b-89e3-0aa8a4a9a290" />

IAM Permissions Policy
<img width="1912" height="962" alt="Screenshot 2026-03-12 081506" src="https://github.com/user-attachments/assets/ea1129f1-78c7-47cd-ba3d-f05bf6629583" />

### Step 3 — Lambda Function
1. Create a Lambda function named `NetQuizHandler` (Python 3.12)
2. Assign the `NetQuizRole`
3. Paste the quiz handler code and update `BUCKET_NAME` with your bucket name
4. Click **Deploy**, then test with the `StartQuizTest` event

<img width="1890" height="854" alt="Screenshot 2026-03-12 075005" src="https://github.com/user-attachments/assets/53668a48-bf14-4752-a0af-22644bfd8d5f" />

<img width="1892" height="910" alt="Screenshot 2026-03-12 080006" src="https://github.com/user-attachments/assets/61d38e07-ba60-492d-bdc1-d2e8792b1ca9" />


6. Test Event
<img width="1770" height="850" alt="test" src="https://github.com/user-attachments/assets/258056ab-373b-47c9-ba21-a30a2104d9fd" />

### Step 4 — Amazon Lex Bot
1. Create a bot named `VPCQuizBot`
2. Create intent **`StartQuiz`** with utterances: *start quiz, begin quiz, quiz me*, etc.
3. Create custom slot type **`QuizAnswerType`** with values: `A`, `B`, `C`, `D`
4. Create intent **`VPCQuiz`** with slot **`UserAnswer`** (type: `QuizAnswerType`)
5. Link Lambda under **Aliases → TestBotAlias → English → VPCQuizHandler**
6. **Build** the bot, then **Test** in the Lex console

<img width="1886" height="940" alt="Screenshot 2026-03-12 112941" src="https://github.com/user-attachments/assets/409e3016-2377-4fb8-b552-8f93354aea07" />

<img width="1914" height="951" alt="Screenshot 2026-03-12 084218" src="https://github.com/user-attachments/assets/24e972b8-8c02-4f30-8f14-8d2d49a1a695" />

<img width="1919" height="891" alt="Screenshot 2026-03-12 133716" src="https://github.com/user-attachments/assets/3746a6de-eb91-4bd8-8c81-fa14c374b200" />

<img width="1895" height="913" alt="Screenshot 2026-03-12 084028" src="https://github.com/user-attachments/assets/fcf462a6-1f89-42e5-9b53-6130159dce08" />

## Sample Conversation

User:  start quiz
Bot:   Welcome to the AWS VPC Quiz!**

<img width="344" height="655" alt="Screenshot 2026-03-12 112351" src="https://github.com/user-attachments/assets/d658e291-53e9-4c48-bfac-7244b71cfe1e" />

       Question 1 (Beginner):
       What does CIDR represent?

       A) DNS routing format
       B) IP address range notation
       C) Encryption method
       D) Load balancing strategy

       Type A, B, C, or D to answer.
       
<img width="344" height="655" alt="Screenshot 2026-03-12 112333" src="https://github.com/user-attachments/assets/e4b8b5e9-898e-42cc-a48f-f3679a71220c" />

<img width="344" height="655" alt="Screenshot 2026-03-12 112311" src="https://github.com/user-attachments/assets/3fee5366-3bea-4b01-a088-59503df71fbf" />

User:  B
Bot:   CORRECT! CIDR defines IP address ranges such as 10.0.0.0/16.

       Next question:
       Question 2 (Beginner): ...
<img width="344" height="655" alt="Screenshot 2026-03-12 112250" src="https://github.com/user-attachments/assets/6e454d10-47e0-4b77-82c2-b4627acce225" />

<img width="344" height="655" alt="Screenshot 2026-03-12 112223" src="https://github.com/user-attachments/assets/d01e03bf-3224-4dc6-b021-e5bd458a234d" />

<img width="344" height="655" alt="Screenshot 2026-03-12 112202" src="https://github.com/user-attachments/assets/71a301f1-1769-4ce9-85f3-3d3c3341d1ce" />


Bot:   Quiz complete! You scored 13/15. Well done!

<img width="344" height="655" alt="Screenshot 2026-03-12 105456" src="https://github.com/user-attachments/assets/a90e84ac-535b-41f8-94e7-0926af5c88a8" />


## Troubleshooting

| Issue | Fix |
|---|---|
| Lambda: Access Denied on S3 | Confirm `LambdaLexQuizRole` has `AmazonS3ReadOnlyAccess` and bucket name in code is exact match |
| Lex gives generic / no response | Click **Deploy** in Lambda, then **rebuild** the Lex bot. Check Lambda is linked under Aliases |
| Bot repeats the same question | Verify Lambda returns `sessionAttributes` (including `questionIndex`) in every response |
| Slot value is `None` in Lambda | Slot name `UserAnswer` must match exactly in both Lex and Lambda (case-sensitive) |
| JSON parse error in Lambda | Validate `quiz_questions.json` at [jsonlint.com](https://jsonlint.com) before re-uploading |
| Lex doesn't recognise utterance | Add more utterance variations to the intent and rebuild |


## Security Highlights

- S3 bucket has **Block All Public Access** enabled — no public exposure of quiz data
- Lambda accesses S3 exclusively via **IAM role** with least-privilege read-only permissions
- Lex invokes Lambda via a **resource-based policy** (auto-configured during setup)


## What I Learned

- How Amazon Lex **intents**, **utterances**, and **slots** work together to manage conversation state
- How to use **session attributes** in Lex to persist data (score, question index) across conversation turns
- How to write a Lambda function that returns **Lex V2-formatted responses** (`ElicitSlot`, `Close`)
- How IAM roles enable **secure, scoped access** between AWS services without public credentials
- How to build and debug a **multi-service serverless architecture** on AWS


## Author

**Stacey**
Praesignis AWS re/Start Programme
Built for CloudLearners Inc. — Project 3


## Licence

This project was built for educational purposes as part of a structured AWS training programme.
