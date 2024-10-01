# S3
- ** Amazon S3 (Simple Storage Service) is a scalable object storage service provided by AWS (Amazon Web Services) designed for storing and retrieving any amount of data from anywhere on the web. 

## Step 1: Create an S3 Bucket
1. **Log in to the AWS Management Console** and navigate to the S3 service.
2. Click on **"Create bucket."**
3. Fill in the **bucket name** and select the region.
4. Configure **bucket settings** as needed (e.g., versioning, encryption).
5. Click **"Create bucket."**


# Amazon S3 Storage Classes

Amazon S3 offers various storage classes optimized for different use cases, balancing access frequency, durability, availability, and cost.

## 1. S3 Standard
- **Use Case:** Frequently accessed data.
- **Availability:** 99.99%
- **Durability:** 99.999999999% (11 9's)
- **Key Features:** 
  - Low latency and high throughput.
  - Suitable for performance-sensitive applications like websites, content distribution, and big data analytics.
- **Cost:** Higher due to optimization for frequent access.

## 2. S3 Intelligent-Tiering
- **Use Case:** Data with unpredictable or changing access patterns.
- **Availability:** 99.9% or 99.99% depending on access tier.
- **Durability:** 99.999999999% (11 9's)
- **Key Features:** 
  - Automatically moves data between frequent and infrequent access tiers based on access patterns.
  - No retrieval fees.
- **Cost:** Slightly higher than Standard-IA but lower when data is infrequently accessed.

## 3. S3 Standard-IA (Infrequent Access)
- **Use Case:** Infrequently accessed data that needs quick retrieval.
- **Availability:** 99.9%
- **Durability:** 99.999999999% (11 9's)
- **Key Features:** 
  - Lower storage cost compared to S3 Standard.
  - Retrieval fee applies.
  - Suitable for backups, disaster recovery, and long-term storage.
- **Cost:** Lower storage cost with additional retrieval and data transfer fees.

## 4. S3 One Zone-IA
- **Use Case:** Infrequently accessed data that doesn’t require multi-AZ resilience.
- **Availability:** 99.5%
- **Durability:** 99.999999999% (11 9's)
- **Key Features:** 
  - Data is stored in a single AZ, offering lower cost.
  - Suitable for easily reproducible data.
- **Cost:** Lower than Standard-IA but with similar retrieval fees.

## 5. S3 Glacier
- **Use Case:** Long-term archival storage for rarely accessed data.
- **Availability:** 99.99%
- **Durability:** 99.999999999% (11 9's)
- **Key Features:** 
  - Extremely low storage cost.
  - Higher latency for data retrieval (minutes to hours).
  - Suitable for long-term backups, regulatory archives, and compliance data.
- **Cost:** Very low storage cost with higher retrieval cost based on retrieval speed.

## 6. S3 Glacier Deep Archive
- **Use Case:** Long-term, cold storage with very rare access needs.
- **Availability:** 99.99%
- **Durability:** 99.999999999% (11 9's)
- **Key Features:** 
  - Lowest cost storage class.
  - Retrieval times can take 12+ hours.
  - Designed for data retention for 7-10 years or longer.
- **Cost:** Lowest storage cost among all S3 classes.

## 7. S3 Outposts
- **Use Case:** Storing data locally for low-latency applications in on-premises environments.
- **Availability:** Same as the AWS region's standard availability.
- **Durability:** Same as S3 Standard in the region where Outposts is deployed.
- **Key Features:** 
  - Extends S3 storage to on-premises environments for local data processing.
  - Integrates with AWS cloud services.
- **Cost:** Includes costs associated with Outposts infrastructure and S3 storage.

## 8. S3 Reduced Redundancy Storage (RRS)
- **Use Case:** Non-critical, reproducible data (Note: This is deprecated for new objects).
- **Availability:** N/A (Discontinued for new objects).
- **Durability:** 99.99%
- **Key Features:** 
  - Previously used for easily replaceable data.
  - Deprecated in favor of other storage classes.
- **Cost:** Cheaper due to reduced durability.

## Choosing the Right Storage Class
- **Frequent Access:** Use S3 Standard or Intelligent-Tiering.
- **Infrequent Access:** S3 Standard-IA or One Zone-IA.
- **Archival:** S3 Glacier or Glacier Deep Archive.
- **Local Processing:** S3 Outposts.

These classes enable you to optimize costs, availability, and performance based on your specific storage needs.


# Amazon S3 Glacier Storage Classes

Amazon S3 offers two main Glacier storage classes designed for long-term archival and infrequently accessed data. Below is a detailed explanation of each.

## 1. S3 Glacier

### Purpose
S3 Glacier is ideal for data that is rarely accessed but must be stored for long periods with flexible retrieval times.

### Durability
- **Durability**: 99.999999999% (11 nines) durability, meaning your data is highly unlikely to be lost.

### Retrieval Options
- **Expedited**: Retrieve data within 1-5 minutes. Suitable for urgent needs.
- **Standard**: Retrieve data within 3-5 hours. The default option for general retrieval.
- **Bulk**: Retrieve large amounts of data within 5-12 hours at the lowest cost.

### Use Cases
- Archival of large data sets.
- Backup and disaster recovery.
- Regulatory and compliance data storage.

### Cost
S3 Glacier offers very low storage costs, but there are fees associated with data retrieval and requests. Pricing varies based on the retrieval option chosen (Expedited, Standard, or Bulk).

## 2. S3 Glacier Deep Archive

### Purpose
S3 Glacier Deep Archive is Amazon’s lowest-cost storage class, designed for data that is very rarely accessed, with retrieval times measured in hours or more.

### Durability
- **Durability**: 99.999999999% (11 nines) durability, ensuring data is preserved over the long term.

### Retrieval Options
- **Standard**: Retrieve data within 12 hours.
- **Bulk**: Retrieve data within 48 hours at the lowest cost.

### Use Cases
- Long-term data archiving.
- Data retention for regulatory compliance.
- Archival of data that is accessed less than once a year.

### Cost
S3 Glacier Deep Archive has the lowest cost of any Amazon S3 storage class. Similar to S3 Glacier, it involves retrieval and request fees, with higher costs for faster retrieval.

## Key Differences Between S3 Glacier and S3 Glacier Deep Archive

- **Retrieval Times**: S3 Glacier offers faster retrieval options (Expedited and Standard) compared to Glacier Deep Archive, which is optimized for much longer retrieval times.
- **Cost**: S3 Glacier Deep Archive is cheaper than S3 Glacier, making it more suitable for data that is accessed extremely infrequently.

## Use Cases Summary

- **S3 Glacier**: Suitable for data that may need to be accessed occasionally with some flexibility in retrieval times.
- **S3 Glacier Deep Archive**: Ideal for long-term archival storage where data is almost never accessed, and retrieval times of 12 hours or more are acceptable.

Both S3 Glacier and S3 Glacier Deep Archive provide cost-effective solutions for storing large amounts of data for extended periods, with varying levels of retrieval urgency.

# Amazon S3 Lifecycle Rules

Amazon S3 lifecycle rules allow you to automate the management of objects within your S3 buckets, optimizing costs and ensuring compliance with data retention policies.

## Key Components of S3 Lifecycle Rules

### 1. Transition Actions
- **Purpose:** Automatically move objects to a different storage class based on their age or a specified date.
- **Examples:**
  - Move objects to **S3 Standard-IA** after 30 days.
  - Move objects to **S3 Glacier** after 90 days.
  - Move objects to **S3 Glacier Deep Archive** after 180 days.

### 2. Expiration Actions
- **Purpose:** Automatically delete objects after a specified period of time.
- **Examples:**
  - Delete objects that are older than 365 days.
  - Remove previous versions of objects (if versioning is enabled) after 90 days.
  - Permanently delete expired delete markers or incomplete multipart uploads.

### 3. Abort Incomplete Multipart Uploads
- **Purpose:** Automatically delete incomplete multipart uploads after a specified number of days.
- **Example:** Abort incomplete multipart uploads that are older than 7 days to save on storage costs.

## Configuring S3 Lifecycle Rules

Lifecycle rules can be configured through the S3 Management Console, AWS CLI, or AWS SDKs. Below are the steps for setting up lifecycle rules in the S3 Management Console:

### 1. Access the S3 Management Console
- Log in to the AWS Management Console and navigate to the S3 service.

### 2. Choose Your Bucket
- Select the bucket for which you want to set up lifecycle rules.

### 3. Navigate to the "Management" Tab
- Within the bucket, go to the "Management" tab where you'll find the "Lifecycle rules" section.

### 4. Create a New Lifecycle Rule
- Click on "Create lifecycle rule."
- Provide a name for the rule and specify whether the rule should apply to all objects in the bucket or to specific prefixes/tags.

### 5. Set Up Transition Actions
- Specify the storage class transitions for your objects. For example, move objects to **S3 Standard-IA** after 30 days, and to **S3 Glacier** after 90 days.

### 6. Set Up Expiration Actions
- Define when objects should expire (e.g., delete objects after 365 days).
- If versioning is enabled, you can also specify when to delete previous versions or expired delete markers.

### 7. Abort Incomplete Multipart Uploads (Optional)
- Enable and configure this option if you want to automatically delete incomplete multipart uploads after a certain number of days.

### 8. Review and Save the Rule
- Review the rule settings and save it. The rule will now automatically apply to the objects in your bucket.

## Example Lifecycle Rule Configuration (JSON)

Here's an example of a JSON configuration for an S3 lifecycle rule:

```json
{
    "Rules": [
        {
            "ID": "TransitionToIA",
            "Prefix": "",
            "Status": "Enabled",
            "Transitions": [
                {
                    "Days": 30,
                    "StorageClass": "STANDARD_IA"
                },
                {
                    "Days": 90,
                    "StorageClass": "GLACIER"
                }
            ],
            "Expiration": {
                "Days": 365
            },
            "AbortIncompleteMultipartUpload": {
                "DaysAfterInitiation": 7
            }
        }
    ]
}
```
# AWS S3 Security: IAM Policies vs. Bucket Policies vs. ACLs

## Overview

AWS S3 security can be managed using three main methods: IAM policies, bucket policies, and ACLs (Access Control Lists). Each of these approaches offers different levels of control and flexibility, and they are often used in combination to achieve the desired security posture.

---

## 1. IAM Policies

### Description
IAM (Identity and Access Management) policies define permissions for AWS identities (users, groups, roles) across all AWS services, including S3. These are attached to IAM identities and specify what actions they can perform on specific resources.

### Use Cases
- Control access to S3 resources for individual users, groups, or roles.
- Implement organization-wide policies that apply to multiple services, not just S3.
- Enforce fine-grained permissions across different AWS services.

### Example
An IAM policy that allows a user to list all buckets and read objects in a specific bucket:

```json
{
   "Version": "2012-10-17",
   "Statement": [
       {
           "Effect": "Allow",
           "Action": "s3:ListAllMyBuckets",
           "Resource": "arn:aws:s3:::*"
       },
       {
           "Effect": "Allow",
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::my-bucket/*"
       }
   ]
}

```
## 2. Bucket Policies

### Description
Bucket policies are resource-based policies that are attached directly to a specific S3 bucket. They define what actions are allowed or denied for users and AWS accounts on that bucket and its objects.

### Use Cases
- Grant or deny permissions to an entire bucket or specific objects within the bucket.
- Enable cross-account access by granting permissions to other AWS accounts.
- Enforce security controls like requiring MFA (Multi-Factor Authentication) for certain actions.

### Example
A bucket policy that allows another AWS account to read objects from your bucket:

```json
{
   "Version": "2012-10-17",
   "Statement": [
       {
           "Effect": "Allow",
           "Principal": {
               "AWS": "arn:aws:iam::111122223333:root"
           },
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::my-bucket/*"
       }
   ]
}
```
## Access Control Lists (ACLs) in AWS S3


Access Control Lists (ACLs) are a legacy method of controlling access to S3 buckets and objects. ACLs are attached to individual buckets or objects and define which AWS accounts or groups have what permissions.

## Use Cases

- Granting simple, object-level permissions.
- Managing access for legacy systems that still rely on ACLs.
- Allowing access to predefined Amazon S3 groups like `all-users` or `authenticated-users`.

## Example

An ACL that makes an object publicly readable:

```bash
aws s3api put-object-acl --bucket my-bucket --key myfile.txt --acl public-read
