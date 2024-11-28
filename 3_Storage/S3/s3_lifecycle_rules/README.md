# Amazon S3 Lifecycle Rules Overview

Lifecycle rules in Amazon S3 allow you to automatically manage the lifecycle of objects in your S3 buckets. These rules help to transition objects between different storage classes, expire (delete) objects, and remove old versions, based on specified conditions like age, creation date, or last accessed time. This helps optimize storage costs, especially when handling large amounts of data with varying access needs.

## Key Concepts

- **Transition**: Moving objects between S3 storage classes (e.g., from S3 Standard to S3 Glacier).
- **Expiration**: Automatically deleting objects after a specified period.
- **Versioning**: Managing lifecycle actions for object versions, including current and previous versions.
- **Noncurrent Expiration**: Specifying the removal of old versions of objects when versioning is enabled.
- **Multipart Upload Expiration**: Cleaning up incomplete multipart uploads after a specified time.

---

## Lifecycle Rule Workflow

### 1. Define Lifecycle Rules

- You can create rules at the bucket level or object level (using object prefixes or tags).
- Rules define which objects the lifecycle applies to and what actions should be taken (transition, expiration, etc.).

### 2. Set Lifecycle Actions

Lifecycle actions define what happens to the objects:

- **Transition to another storage class**: Move objects to more cost-efficient storage classes as they age.
- **Expire objects**: Automatically delete objects after a certain time.
- **Purge old versions**: Clean up previous versions of objects when versioning is enabled.
- **Purge incomplete multipart uploads**: Remove incomplete uploads after a specific time.

### 3. Specify Timelines

Lifecycle rules are based on object age. For example, you might move an object to the S3 Standard-IA storage class after 30 days of no access and then to S3 Glacier after 60 days.

### 4. Monitoring and Applying Lifecycle Policies

Once the rules are applied, S3 runs them daily to evaluate and execute the defined actions. Lifecycle transitions are not immediate but usually take place within 24 hours after the object meets the rule criteria.

---

## Lifecycle Rule Use Cases

- **Data Archival**: When you have data that is frequently accessed initially but rarely accessed over time, lifecycle rules can transition data from S3 Standard to S3 Glacier or S3 Glacier Deep Archive to reduce costs.
  - **Example**: Move logs from S3 Standard to S3 Glacier after 90 days and delete them after 365 days.
  
- **Infrequent Access Data**: For objects that are accessed infrequently after a certain period, transition them from S3 Standard to S3 Standard-IA or S3 Intelligent-Tiering.
  - **Example**: Move media files from S3 Standard to S3 Standard-IA after 30 days.

- **Delete Expired Data**: Automatically delete objects when they are no longer needed, such as temporary files, old backups, or versions of objects.
  - **Example**: Delete temporary backup files after 7 days to free up space.

- **Versioning and Cleanup**: In buckets with versioning enabled, you may want to delete old object versions after a set period while retaining the current version.
  - **Example**: Delete previous versions of objects that are more than 90 days old.

- **Incomplete Multipart Upload Cleanup**: Clean up incomplete multipart uploads that were never completed, reducing storage costs for unused data.
  - **Example**: Automatically remove incomplete multipart uploads that are older than 7 days.

---

## How to Set Up Lifecycle Rules in AWS S3

### 1. Using AWS Console

1. Go to the **S3 Console**.
2. Select the bucket where you want to create lifecycle rules.
3. Navigate to the **Management** tab.
4. Click **Add Lifecycle Rule**.
5. Enter a **rule name** and specify the scope (apply to all objects or use a prefix or tags to target specific objects).
6. Choose the **lifecycle actions**: transition objects, expire objects, manage noncurrent versions, or expire incomplete multipart uploads.
7. Define the **time period** for each action (e.g., transition after 30 days, delete after 365 days).
8. Review and save the rule.

### 2. Using AWS CLI

You can also manage lifecycle rules using the AWS CLI. Here's an example to add a lifecycle configuration to transition objects to S3 Glacier after 90 days and expire after 365 days.

```bash
aws s3api put-bucket-lifecycle-configuration --bucket my-bucket --lifecycle-configuration '{
  "Rules": [
    {
      "ID": "MoveToGlacier",
      "Filter": {
        "Prefix": ""
      },
      "Status": "Enabled",
      "Transitions": [
        {
          "Days": 90,
          "StorageClass": "GLACIER"
        }
      ],
      "Expiration": {
        "Days": 365
      }
    }
  ]
}'
```
# S3 Lifecycle Actions and Best Practices

## Lifecycle Actions Breakdown

### 1. Transition Actions

You can specify the number of days after object creation to transition it to another storage class. For example, you can transition objects after 30 days to S3 Standard-IA or 60 days to S3 Glacier.

#### Available storage classes for transitions:

- **S3 Standard-IA** (Infrequent Access)
- **S3 Intelligent-Tiering**
- **S3 One Zone-IA**
- **S3 Glacier Instant Retrieval**
- **S3 Glacier Flexible Retrieval**
- **S3 Glacier Deep Archive**

### 2. Expiration Actions

- You can set the expiration period to delete objects after a specified number of days.
- For versioned buckets, you can set noncurrent versions to expire after a specified time.

### 3. Incomplete Multipart Upload Actions

- You can clean up incomplete multipart uploads after a set number of days to avoid unnecessary storage costs.

---

## Example Workflow: Log Archiving

### Scenario:

You are storing web server logs in S3, and you want to:

1. Store them in the **S3 Standard** class for the first 30 days.
2. Transition them to **S3 Glacier Flexible Retrieval** for archival storage after 90 days.
3. Delete them after 365 days.

### Step-by-Step Workflow:

- **Day 0-30**: Logs are stored in **S3 Standard** for frequent access.
- **Day 31-90**: Logs are automatically transitioned to **S3 Standard-IA** as access frequency decreases.
- **Day 91-365**: Logs are transitioned to **S3 Glacier Flexible Retrieval** for long-term storage.
- **Day 366**: Logs are deleted, freeing up storage space.

---

## Best Practices for S3 Lifecycle Rules

1. **Analyze Access Patterns**:  
   Understand the data access patterns and design lifecycle policies to match your needs. For example, frequently accessed data should remain in S3 Standard or S3 Intelligent-Tiering, while infrequently accessed data can transition to lower-cost storage.

2. **Test in a Sandbox**:  
   Apply lifecycle policies in a test environment to avoid accidental deletion or incorrect transitions. This ensures that your rules behave as expected before applying them to production data.

3. **Use Tags**:  
   Organize data with S3 object tags for more granular control over lifecycle management. Tags can help target specific objects for different lifecycle rules within the same bucket.

4. **Monitor Costs**:  
   Use **AWS Cost Explorer** to track how lifecycle transitions are affecting your storage costs. Make sure transitions to lower-cost storage classes are effectively reducing costs.

5. **Use Multiple Rules**:  
   You can define multiple rules in a single lifecycle configuration to handle different types of objects differently. For example, you can have one rule for logs and another for media files, each with their own transition and expiration schedules.

---

By following these best practices and understanding the lifecycle actions available in S3, you can optimize your storage costs and ensure efficient data management.
