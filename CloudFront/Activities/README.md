
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



   # How to make EC2/ALB Instance Accessible from CloudFront Only

   1. Go to VPC > Managed prefix lists >  com.amazonaws.global.cloudfront.origin-facing - get prefix id 
   2. Go to instance Security Group and allow HTTP -  from above prefix id