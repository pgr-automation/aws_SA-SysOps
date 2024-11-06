# AWS CloudFront Logging and Reporting

**CloudFront Logs** provide detailed information about requests made to your CloudFront distribution. By enabling logging, you can gather data on who is accessing your content, what content they’re accessing, and how CloudFront is performing. Logs are useful for debugging, analytics, and security monitoring.

---

## Benefits of CloudFront Logging

- **Traffic Insights**: Understand where your requests are coming from and the performance of your CloudFront distribution.
- **Error Monitoring**: Track HTTP errors such as 404s or 503s and pinpoint performance bottlenecks.
- **Security Auditing**: Analyze access patterns to identify potential security issues, such as unauthorized access attempts.
- **Usage Reporting**: Track content delivery for auditing purposes or for reporting on usage.

---

## Enabling CloudFront Access Logging

### Step 1: Create an S3 Bucket for Logs

1. Open the **S3 Console** and create a new S3 bucket where CloudFront will store the logs.
   - Example bucket name: `my-cloudfront-logs-bucket`.
   - Ensure the bucket has appropriate permissions to allow CloudFront to write logs to it.

2. Grant CloudFront the necessary permissions to write logs to the S3 bucket:
   - Navigate to the **Bucket Policy** section.
   - Add the following policy, replacing `my-cloudfront-logs-bucket` with your actual bucket name:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CloudFrontLogging",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::my-cloudfront-logs-bucket/*"
    }
  ]
}
```

This guide provides a detailed walkthrough for enabling and analyzing **AWS CloudFront Logging**. It covers enabling CloudFront logging, verifying logs, and analyzing logs using various AWS tools like CloudWatch and Athena, as well as other third-party analytics tools.

---

### Step 2: Enable Logging in CloudFront

1. **Go to the CloudFront Console.**
2. **Select the CloudFront Distribution** where you want to enable logging.
3. **In the General settings**, click **Edit**.
4. **Under the Logging section:**
   - Set **Enable Logging** to **Yes**.
   - Specify the **S3 bucket** for logs (the S3 bucket you created earlier).
   - Optionally, define a **Log Prefix** to organize logs into folders within the S3 bucket.
5. **Click Yes, Edit** to save changes.



### Step 3: Verify Logging is Working

After enabling logging, CloudFront will start writing log files to the specified S3 bucket. Note that logs may take up to 30 minutes to appear. Check the S3 bucket for log files with the following naming convention:


---

### CloudFront Log File Format

CloudFront log files contain detailed information about each request, including:

- **Date**: The date and time of the request.
- **Edge Location**: The CloudFront edge location that served the request.
- **Client IP**: The IP address of the user making the request.
- **Request URI**: The requested resource URL.
- **HTTP Status Code**: The response status code (e.g., 200, 404, 503).
- **Referrer**: The referring URL (if available).
- **User Agent**: The browser or client making the request.
- **Cache Status**: Whether the request was a cache hit or miss.





---

## Analyzing CloudFront Logs

You can analyze CloudFront logs in several ways:

### Option 1: Amazon Athena

You can query CloudFront logs stored in S3 using Amazon Athena for real-time analysis.

1. Open the **Athena Console** and create a new database.
2. Define a **table schema** for CloudFront logs (pre-built templates are available for CloudFront logs).
3. Run **SQL queries** to analyze access patterns, top requests, error rates, and more.

### Option 2: Amazon CloudWatch Logs Insights

You can stream CloudFront logs into CloudWatch Logs for easier querying and integration with other AWS services. This setup allows for real-time reporting and alerting.

### Option 3: Third-Party Analytics Tools

Use third-party tools like **Google Analytics**, **Splunk**, or **ELK Stack** to analyze and visualize CloudFront logs.

### Option 4: AWS QuickSight

AWS QuickSight is a business intelligence tool that can connect to your CloudFront logs stored in S3 or CloudWatch for visual analytics and reporting.

---

## CloudFront Metrics and Reporting with CloudWatch

CloudFront automatically integrates with Amazon CloudWatch to provide key metrics for your distribution:

- **Requests**: The total number of requests.
- **Bytes Downloaded**: Total data transferred to users.
- **Bytes Uploaded**: Total data uploaded from users.
- **Cache Hits**: Percentage of requests served from CloudFront cache.
- **4xx and 5xx Errors**: Number of client and server errors.
- **Latency**: Time taken to serve the content from edge locations.

### To View Metrics in CloudWatch

1. Go to the **CloudWatch Console**.
2. Navigate to **Metrics > CloudFront**.
3. Select the metrics you want to analyze (e.g., Requests, 4xx Errors, etc.).

You can set up **CloudWatch Alarms** to monitor CloudFront performance and get notified of specific thresholds, such as high error rates or sudden traffic spikes.

---

## Example Use Cases for Logging

- **Content Delivery Analytics**: Track how often certain resources (e.g., images, videos) are accessed and optimize content delivery.
- **Troubleshooting**: Investigate performance issues by analyzing cache hit/miss ratios and response codes.
- **Security Monitoring**: Detect unusual access patterns or potentially malicious activity by reviewing logs for suspicious IPs or user agents.
- **Compliance Auditing**: Maintain logs for auditing purposes to ensure compliance with regulatory requirements.

---

## Additional Resources

- [AWS CloudFront Logging Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)
- [Amazon Athena Documentation](https://docs.aws.amazon.com/athena/latest/ug/what-is.html)
- [AWS CloudWatch Metrics for CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/monitoring-using-cloudwatch.html)

---

> **Note**: Be mindful of the storage costs for S3 logs and the additional costs associated with using services like CloudWatch and Athena. Optimize log retention policies to control costs.



# AWS CloudFront Invalidation Guide

This guide explains how to remove cached data from AWS CloudFront edge locations by performing an invalidation. This process clears specific files or entire paths from the CloudFront cache, forcing it to fetch updated content from the origin server.

## 1. Invalidate Files Using the AWS Management Console

1. Log in to the [CloudFront Console](https://console.aws.amazon.com/cloudfront/).
2. Choose your **CloudFront distribution** from the list.
3. Select the **Invalidations** tab.
4. Click on **Create Invalidation**.
5. Enter the path(s) of the objects to invalidate:
   - For a specific file: `/path/to/your/file.jpg`
   - For all files (use cautiously, as it may incur costs): `/*`
6. Click **Invalidate** to submit the invalidation request.

## 2. Invalidate Files Using AWS CLI

To automate invalidation, use the AWS CLI:

```bash
aws cloudfront create-invalidation --distribution-id YOUR_DISTRIBUTION_ID --paths "/*"
```
## 3. Important Notes

   *  Costs: The first 1,000 invalidation paths per month are free; additional invalidations incur a cost.
   *  Time: Invalidations may take several minutes to propagate globally.
   *  Wildcard Paths: Use /* with caution as it will invalidate all objects, which may be costly and impact performance.

