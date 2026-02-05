AHKU Cafe – AWS Cloud Solution Depolyment (Demo)

Project Overview
This document outlines the process used to host the AHKU Cafe static website using Amazon S3 Static Website Hosting as part of the AWS re/Start Portfolio Project. Amazon S3 was selected as the hosting solution due to its cost-effectiveness, high availability, scalability, and serverless nature, making it an ideal choice for a small café business seeking a reliable online presence with minimal operational overhead.

Prerequisites
- Before beginning the deployment, ensure the following requirements are met:
- An active AWS account
- A completed static website (HTML/CSS files)
- A main website file named index.html
  

### Step 1: Log in to the AWS Management Console
- Open a web browser and navigate to https://aws.amazon.com
- Click Sign in to the Console
- Log in using your AWS account credentials

<img width="1319" height="842" alt="1" src="https://github.com/user-attachments/assets/06a93532-fc64-4b0d-8040-eb7919bdddba" />

### Step 2: Access Amazon S3
- In the AWS Management Console search bar, type S3
- Select Amazon S3 from the results
- You will be redirected to the S3 dashboard

<img width="438" height="276" alt="2" src="https://github.com/user-attachments/assets/cb6ac3a8-69fe-47ef-9d70-9dd0b00ec614" />

### Step 3: Create an S3 Bucket
- Click Create bucket
- Enter a globally unique bucket name: ahku-cafe-website
- Select a Region: Africa (Cape Town)
- Leave Object Ownership set to the default option (ACLs disabled)

<img width="1267" height="572" alt="6" src="https://github.com/user-attachments/assets/998607b0-b97f-41ad-846e-08f3fad551f5" />

### Step 4: Configure Public Access Settings
- Under Block Public Access settings, uncheck:
- Block all public access
- Acknowledge the warning by selecting the confirmation checkbox
- Leave Bucket Versioning disabled
- Leave Default Encryption enabled (SSE-S3)
- Click Create bucket

<img width="1605" height="684" alt="json codepng" src="https://github.com/user-attachments/assets/867bb307-bda7-4980-be95-9c6e5b9f20aa" />

### Step 5: Upload Website Files
- Open the newly created bucket
- Click Upload
- Select Add files
- Upload index.html and any additional assets (such as images, CSS, or JavaScript files)
- Click Upload

<img width="1834" height="571" alt="8" src="https://github.com/user-attachments/assets/ba7f1946-e5d8-4b0f-9e21-ddfa0cb93e95" />

###  Step 6: Enable Static Website Hosting
- Navigate to the Properties tab of the bucket
- Scroll down to Static website hosting
- Click Edit
- Select Enable
- Choose Host a static website
- Enter the following details:
- Index document: index.html
- Click Save changes

<img width="1834" height="735" alt="12" src="https://github.com/user-attachments/assets/5be5d603-13cb-42b5-89b1-54af57e4e6c7" />

### Step 7: Configure Bucket Policy for Public Access
- Go to the Permissions tab
- Scroll to Bucket policy and click Edit
- Paste the following policy (update the bucket name if required):

<img width="1605" height="684" alt="json codepng" src="https://github.com/user-attachments/assets/aee52808-fe25-465d-917d-e207ae200daf" />

- Click Save changes

### Step 8: Access the Live Website
- Return to the Properties tab
- Scroll to Static website hosting
- Copy the Bucket website endpoint
- Paste the URL into a web browser to view the live AHKU Cafe website

http://ahku-cafe-website.s3-website.af-south-1.amazonaws.com

### Benefits of Hosting AHKU Cafe on Amazon S3
- Low Cost: Pay only for storage and usage
- High Availability: 99.99% availability
- Scalability: Automatically handles traffic growth
- No Server Management: Infrastructure is fully managed by AWS
- Security: Supports encryption and integration with AWS IAM

Conclusion
Hosting the AHKU Cafe website on Amazon S3 provides the business with a reliable, scalable, and cost-effective online presence. This solution eliminates the need for on-premises servers while enhancing accessibility, performance, and customer engagement—making it well suited for a growing café business.




Shorten it for a presentation or portfolio summary


