# Secure-an-AWS-S3-Bucket-for-Public-and-Private-Access

A secure AWS storage architecture to locked-down S3 buckets, CloudFront via Origin Access Control (OAC), dynamic pre-signed URLs, and continuous CloudTrail auditing. Designed for safe public content delivery and strict data privacy.
AWS S3 Secure Architecture with CloudFront and Origin Access Control
A step-by-step guide to create a private Amazon S3 bucket, block all public access, and serve content securely through CloudFront with Origin Access Control (OAC) and CloudTrail monitoring.

Context

Organizations need to serve static content and files from Amazon S3 while preventing accidental public exposure and maintaining complete audit trails of data access. This guide walks through securing S3 buckets behind CloudFront using Origin Access Control (OAC), ensuring only authenticated CloudFront distributions can access your data, and implementing continuous monitoring with CloudTrail. It's designed for AWS users of any skill level who want security without complexity.

Prerequisites

AWS Account with appropriate IAM permissions (S3, CloudFront, CloudTrail, CloudWatch
AWS CLI v2.x or higher (optional, for pre-signed URLs)
AWS Console access (all steps work through the web interface)
Existing or new S3 bucket (this guide covers bucket creation)
Installation
No software installation is required. All configuration happens in the AWS Console.

Usage

Create and Secure your S3 Bucket

Create Bucket
1.	Go to S3 Create bucket
2.	Enter bucket name (my-secure-bucket-12345)
3.	Select region
4.	Click Create bucket

Block Public Access
1.	Select your bucket
2.	Go to permissions tab
3.	Scroll to Block public access (bucket settings)
4.	Click Edit
5.	Check all 4 boxes:
    •	✓ Block all public ACLs
    •	✓ Ignore all public ACLs
    •	✓ Block public bucket policies
    •	✓ Block public and cross account access
6.	Click Save changes
 <img width="959" height="383" alt="Edit Block Public Access" src="https://github.com/user-attachments/assets/5096cae2-412b-4730-a719-b40871462876" />

This image allows you to check your work after you follow the directions
Enable Encryption
1.	Go to Properties tab
2.	Scroll to Default encryption
3.	Click Edit
4.	Select SSE-S3 (AES-256)
5.	Click Save changes
<img width="960" height="383" alt="S3 Encryption" src="https://github.com/user-attachments/assets/7f3b6ce3-16a9-436d-9e08-14c43e6261b7" />
 
This image allows you to check your work after you follow the steps 
Enable Versioning
1.	Stay in Properties tab
2.	Scroll to Bucket Versioning
3.	Click Edit
4.	Select Enable
5.	Click Save changes

DDoS Protection (Shield Standard)
1.	Go to AWS Shield (top search bar)
2.	Review Shield Standard details
3.	AWS Shield Standard is automatically enabled at no cost
4.	No additional configuration needed for your S3 bucket
5.	Protection covers Layer 3 & 4 DDoS attacks (network & transport layer)
FYI: Shield Standard is free and automatically active. Shield Advanced provides enhanced DDoS mitigation, 24/7 DDoS Response Team (DRT), and cost $3,000/month minimum protection.

Create CloudFront Distribution
Go to CloudFront
1.	Open the AWS Console and search for CloudFront
2.	Click Distributions in the left sidebar
3.	Click create distribution

Create Origin Access Control (OAC)
AWS CloudFront
1.	Origin Name (S3 bucket)
2.	Click Origin Access Control settings (recommended) 
3.	Origin type: S3
4.	Click create distribution 

Generate a pre-signed URL for temporary access
Step 1: Navigate to Object
1.	Go to your S3 bucket
2.	Click into folders to find your file
3.	Select the file (document.pdf)

Step 2: Generate Pre-Signed URL
1.	Click the file to select it
2.	Click Object actions (top right)
3.	Select Share with a presigned URL
<img width="960" height="382" alt="Pre Signed URL" src="https://github.com/user-attachments/assets/e77f390a-a5f5-44cd-8165-374fbaa4f2a2" />
 
This image allows you to check your work after you follow the steps
Set Expiration Time
1.	In the dialog, set Duration (expiration time):
    •	Minimum: 1 minute
    •	Maximum: 12 hours (43,200 seconds)
2.	Select your desired duration (e.g., 60 minutes)
3.	Click Create presigned URL

Copy & Share
1.	Copy the generated URL from the dialog
2.	Share with intended recipient
3.	URL expires after the specified duration
4.	Expired URLs return Access Denied

Enable CloudTrail for audit logging
Create Trail
1.	Click Create trail (blue button)
2.	Enter Trail name: s3-audit-trail
3.	Under Storage location:
    •	Select Create new S3 bucket
    •	Enter bucket name: cloudtrail-logs-12345
4.	Check ✓ Enable log file SSE-S3 encryption
5.	Check ✓ Enable log file validation
6.	Click Next

Configure Log Events
1.	Under Management events, check:
    •	✓ Write (API calls that modify resources)
    •	✓ Read (API calls that read resources)
2.	Under Data events, click Add data event
3.	Select S3 from dropdown
4.	Choose logging scope:
    •	Select Specific S3 buckets
    •	Enter your bucket name (e.g., my-secure-bucket-12345)
5.	Under Data event type, check:
    •	✓ GetObject (S3 read operations)
    •	✓ PutObject (S3 write operations)
6.	Click Next

Contributing

PRs and bug reports welcome. This is a living guide; if you find outdated AWS console steps, incorrect CLI syntax, or missing security best practices, please open an issue or submit a PR with corrections.

License

This guide is released under the MIT License. Use, modify, and distribute freely with attribution.
See LICENSE file for full details.
