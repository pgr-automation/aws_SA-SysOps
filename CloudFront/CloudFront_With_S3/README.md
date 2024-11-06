# Setting Up AWS CloudFront with S3 Bucket

Using **AWS CloudFront** with an **S3 Bucket** as the origin allows you to deliver static content like images, videos, and HTML files globally with low latency. This setup is ideal for website hosting, content distribution, and caching.

---

## Benefits of Using CloudFront with S3

- **Reduced Latency**: Delivers content from edge locations closer to users, resulting in faster load times.
- **Improved Performance**: Caches content, reducing requests to the origin S3 bucket and improving overall performance.
- **Cost-Effective**: Lowers costs by reducing the number of direct requests to S3.
- **Security**: Integrates with AWS Identity and Access Management (IAM), AWS Shield, AWS WAF, and supports SSL/TLS encryption for secure content delivery.

---

## Setting Up CloudFront with S3 Bucket

### Step 1: Create an S3 Bucket

1. Open the **S3 Console** in AWS.
2. Click on **Create Bucket**.
3. Enter a unique bucket name and select a region.
4. Configure **Public Access Settings**:
   - If you want to make the content public, adjust the public access settings and permissions as required.
5. Upload the content you want to serve (e.g., images, videos, HTML files) to the S3 bucket.

### Step 2: Create a CloudFront Distribution

1. Open the **CloudFront Console** in AWS.
2. Click on **Create Distribution**.
3. Choose **Web** as the distribution type.
4. Under **Origin Settings**:
   - Set **Origin Domain Name** to your S3 bucket.
   - Choose **Restrict Bucket Access** if you want only CloudFront to access your S3 bucket directly.
5. **Viewer Protocol Policy**: Set this to **Redirect HTTP to HTTPS** for secure delivery.
6. Configure **Cache Behavior**:
   - You can define path patterns, caching behaviors, and viewer protocols.
7. (Optional) **SSL Certificate**: Choose an ACM (AWS Certificate Manager) certificate for HTTPS if you’re using a custom domain.
8. Click **Create Distribution**.

### Step 3: Restrict Access to the S3 Bucket (Optional)

To ensure that only CloudFront can access your S3 bucket directly:

1. In the **CloudFront Distribution Settings**, go to **Origins**.
2. Enable **Restrict Bucket Access**.
3. In **S3 Bucket Policy**, create a policy that allows access only from your CloudFront distribution. Example policy:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity <OAI_ID>"
         },
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::<bucket_name>/*"
       }
     ]
   }
```