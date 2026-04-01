 🚀 React App Deployment on AWS (S3 + CloudFront + Route 53 + ACL)

📌 Overview

This project demonstrates how to deploy a React application on AWS using:

- Amazon S3 for static hosting
- CloudFront for CDN and HTTPS
- Route 53 for custom domain
- AWS WAF / ACL for security

---

🏗️ Architecture

User → Route 53 → CloudFront → S3 Bucket

---

⚙️ Tech Stack

- React (Frontend)
- AWS S3 (Static Hosting)
- AWS CloudFront (CDN)
- AWS Route 53 (DNS)
- AWS WAF / ACL (Security)

---

📂 Project Setup

1️⃣ Clone the Repository

git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name

2️⃣ Install Dependencies

npm install

3️⃣ Build the React App

npm run build

This will generate a "build/" folder.

---

☁️ Deployment Steps

🔹 Step 1: Create S3 Bucket

- Go to AWS S3
- Create a bucket with your domain name (e.g., "example.com")
- Disable "Block all public access"
- Enable Static Website Hosting
- Set:
  - Index document: "index.html"
  - Error document: "index.html"

---

🔹 Step 2: Upload Build Files

aws s3 sync build/ s3://your-bucket-name

---

🔹 Step 3: Set Bucket Policy

Allow public access:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}

---

🔹 Step 4: Create CloudFront Distribution

- Origin: S3 bucket
- Viewer Protocol Policy: Redirect HTTP → HTTPS
- Default Root Object: "index.html"

---

🔹 Step 5: Configure Route 53

- Create a hosted zone for your domain
- Add an A record
- Alias → CloudFront distribution

---

🔹 Step 6: Add SSL Certificate

- Use AWS Certificate Manager (ACM)
- Attach certificate to CloudFront

---

🔹 Step 7: Configure WAF (ACL)

- Create Web ACL
- Add rules:
  - Block common attacks (SQL Injection, XSS)
- Attach ACL to CloudFront

---

🔒 Security Features

- HTTPS enabled via CloudFront
- Web ACL for request filtering
- S3 restricted via CloudFront (optional using OAC)

---

🌍 Access Your App

Once deployed, access your app via:

https://your-domain.com
---
🛠️ Troubleshooting
❗ Blank Page Issue
- Ensure "index.html" is set as default root object
- Use CloudFront invalidation:
aws cloudfront create-invalidation --distribution-id YOUR_ID --paths "/*"
❗ 403 Forbidden
- Check S3 bucket policy
- Verify public access settings
---
📌 Future Improvements
- CI/CD using GitHub Actions
- Use S3 + CloudFront with OAC (no public bucket)
- Add monitoring with CloudWatch
---
👨‍💻 Author

Your Name

---
⭐ Contributing

Feel free to fork and contribute!
