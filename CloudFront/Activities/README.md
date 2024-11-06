
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



   # AWS CloudFront Path-Based Routing with Multiple Origins

This guide explains how to set up path-based routing with multiple origins in AWS CloudFront. This setup allows you to route requests based on URL paths to specific origins, enabling you to serve different types of content (like images, videos, and APIs) from distinct backends within a single CloudFront distribution.

## Prerequisites
- An AWS CloudFront distribution.
- Multiple origins configured (such as S3 buckets, EC2 instances, or external HTTP servers).

## Steps to Configure Path-Based Routing

### 1. Define Multiple Origins
1. In the **AWS Management Console**, open your **CloudFront distribution**.
2. Go to the **Origins** section.
3. Add multiple origins, each pointing to a different backend. For example:
   - Origin 1: S3 bucket for images.
   - Origin 2: EC2 instance for APIs.
   - Origin 3: Another S3 bucket for videos.
4. Assign a unique **Origin ID** to each origin for identification.

### 2. Configure Cache Behaviors
1. In the **Behaviors** section of your CloudFront distribution, create cache behaviors for each path pattern.
2. For each cache behavior, specify the **Path Pattern** to determine which requests it applies to. For example:
   - `/images/*` for the S3 bucket serving images.
   - `/api/*` for the EC2 instance serving API requests.
   - `/videos/*` for the S3 bucket serving video content.
3. Assign each cache behavior to the corresponding **Origin**.

### 3. Set Path Pattern and Priority
1. Define specific path patterns using wildcards (`*`) for each cache behavior. Examples:
   - `/images/*` routes to the images S3 bucket.
   - `/api/*` routes to the API EC2 instance.
   - `/videos/*` routes to the video S3 bucket.
2. CloudFront evaluates cache behaviors in order of priority. Arrange them so that more specific paths are evaluated first (e.g., `/api/*` before `/*`).

### 4. Configure Caching and Other Behavior Settings
1. Customize caching settings for each behavior based on the content requirements:
   - Static assets (like images) may have longer cache durations.
   - API responses might have shorter cache durations.
2. Configure additional behavior settings, such as:
   - **Allowed HTTP Methods** (e.g., `GET`, `POST` for APIs).
   - **Viewer Protocol Policy** (e.g., enforce HTTPS).
   - **Origin Request Policy** and **Response Headers Policy** as needed.

### 5. Test and Verify
1. Deploy the CloudFront distribution changes.
2. Test by accessing different paths in your CloudFront URL to confirm requests route to the correct origin:
   - `/images/path-to-image.jpg` should return content from the S3 bucket origin for images.
   - `/api/path-to-endpoint` should hit the EC2 instance.
   - `/videos/path-to-video.mp4` should fetch from the S3 bucket for videos.
3. Use browser developer tools or AWS CloudFront logs to verify that the requests are routed correctly based on path patterns.

## Example Configuration

Here's an example configuration based on common paths:

- **Path Pattern:** `/images/*`
  - **Origin:** S3 bucket for images
  - **Description:** Serves static images from S3.
  
- **Path Pattern:** `/api/*`
  - **Origin:** EC2 instance for API
  - **Description:** Routes API requests to an EC2 instance.

- **Path Pattern:** `/videos/*`
  - **Origin:** S3 bucket for videos
  - **Description:** Serves video content from S3.

## Summary

Using path-based routing in CloudFront helps optimize content delivery by directing requests to the correct origin based on the URL path. This setup is useful for scenarios where you need to serve various types of content from multiple origins under a single CloudFront distribution.

---

For further customization and advanced routing configurations, refer to the [AWS CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web.html).
