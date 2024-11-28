
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




# AWS CloudFront: Cache Expiration and Custom Headers

This guide provides steps to configure cache expiration settings and custom headers in AWS CloudFront. These configurations help optimize content delivery and allow custom metadata to be passed to origins for specific behaviors.

## Prerequisites
- An AWS CloudFront distribution with at least one origin.

---

## Table of Contents
- [Setting Cache Expiration](#setting-cache-expiration)
- [Adding Custom Headers](#adding-custom-headers)
- [Example Configuration](#example-configuration)

---

## Setting Cache Expiration

CloudFront allows you to control the cache expiration settings at the distribution level or on individual cache behaviors. 

### Steps to Configure Cache Expiration
1. **Open CloudFront Distribution Settings**:
   - In the AWS Management Console, navigate to your **CloudFront Distribution**.
   
2. **Edit Cache Behavior Settings**:
   - Go to the **Behaviors** tab.
   - Select the behavior you want to modify, then click **Edit**.

3. **Specify Cache Duration**:
   - Locate the **Object Caching** section.
   - Choose one of the following:
     - **Use Origin Cache Headers**: CloudFront honors the cache headers (like `Cache-Control` or `Expires`) set by the origin.
     - **Customize**: Specify custom TTL (Time to Live) values.
       - **Minimum TTL**: The shortest period objects remain cached, even if cache-control headers allow shorter durations.
       - **Default TTL**: Standard caching time for objects unless overridden by the origin.
       - **Maximum TTL**: Longest time objects remain cached, even if origin headers suggest longer.
   - Save your settings to apply the cache expiration policy.

### Cache Expiration Tips
- **Long TTL for Static Content**: Apply longer cache durations for static assets (e.g., images, CSS files).
- **Short TTL for Dynamic Content**: Use shorter TTLs for frequently changing content (e.g., API responses).
- **Use Versioning**: For static files, consider versioning filenames to force updates without reducing cache efficiency.

---

## Adding Custom Headers

Custom headers are useful when you need to pass additional information to the origin server, such as authentication data, user agents, or custom identifiers.

### Steps to Add Custom Headers
1. **Go to the Origins and Origin Groups**:
   - In the **Origins** section, select the origin you want to configure, then click **Edit**.

2. **Add Custom Headers**:
   - In the **Add Header** section:
     - Specify a **Header Name** (e.g., `X-Custom-Header`).
     - Provide a **Value** for the header.
       - Example: `X-Custom-Header: my-custom-value`
   - You can add multiple headers as needed.

3. **Save Changes**:
   - Save the origin configuration to apply the custom headers.

### Custom Header Tips
- **Authorization Headers**: Pass authorization tokens or API keys for secure origin access.
- **Device Detection**: Send device or user-agent information for responsive content delivery.
- **Debugging**: Use custom headers for debugging purposes in development environments.

---

## Example Configuration

Below is an example configuration for setting cache expiration and custom headers in CloudFront.

### Cache Expiration Example
For an image path pattern (`/images/*`):
- **Minimum TTL**: 1 day
- **Default TTL**: 7 days
- **Maximum TTL**: 30 days

These settings would cache image files on CloudFront edge locations with a default duration of 7 days, optimizing for static content.

### Custom Header Example
For an API path pattern (`/api/*`):
- **Custom Header**: `X-Api-Version: 1.0`
- **Authorization Header**: `Authorization: Bearer <token>`

These headers ensure the backend receives the correct API version information and secure authorization tokens for API calls.

---

## Testing and Verification

1. **Deploy Changes**:
   - After configuring, deploy the distribution settings.

2. **Verify Cache Settings**:
   - Use browser developer tools or CloudFront logs to check the `Cache-Control` and `Expires` headers.

3. **Inspect Custom Headers**:
   - Confirm the presence of custom headers by viewing the request headers sent to the origin using developer tools or monitoring tools.

---

## Conclusion

Configuring cache expiration and custom headers in CloudFront helps tailor content delivery and optimize the performance and security of your application. For more details, refer to the [AWS CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web.html).

---



# AWS CloudFront: Cache Key with Query Parameters

This guide explains how to configure query parameters as part of the cache key in AWS CloudFront. By customizing cache keys, you can optimize caching and ensure unique responses based on query parameters.

## Prerequisites
- An AWS CloudFront distribution with one or more origins.
- Specific query parameters you want to include or exclude from the cache key.

---

## Table of Contents
- [What is a Cache Key?](#what-is-a-cache-key)
- [Configuring Query Parameters in Cache Key](#configuring-query-parameters-in-cache-key)
- [Testing and Verifying](#testing-and-verifying)
- [Example Configuration](#example-configuration)

---

## What is a Cache Key?

A **Cache Key** is a unique identifier that CloudFront uses to determine whether a cached copy of an object can be returned for a request. By default, CloudFront considers several request attributes (like URL path, headers, and cookies) in the cache key. Including query parameters in the cache key allows you to cache different versions of an object based on specific parameter values, improving cache efficiency for dynamic content.

---

## Configuring Query Parameters in Cache Key

1. **Open CloudFront Distribution Settings**:
   - In the AWS Management Console, navigate to **CloudFront**, and select the distribution to configure.

2. **Edit Cache Behavior**:
   - Go to the **Behaviors** tab.
   - Select the cache behavior you want to edit, and click **Edit**.

3. **Configure Cache Key and Origin Requests**:
   - Scroll down to the **Cache Key and Origin Requests** section.
   - Set **Cache Policy** to either:
     - **Use a managed policy**: Select a built-in AWS managed cache policy.
     - **Use a custom policy**: Create a custom cache policy for more granular control.
   
4. **Create or Customize Cache Policy**:
   - If creating a custom policy, select **Create Cache Policy** and configure the following:
     - **Query String Forwarding and Caching**: Choose one of the following options:
       - **None**: Ignore all query parameters.
       - **All**: Cache every unique combination of query parameters.
       - **Only the specified parameters**: Cache based on a defined list of query parameters.
     - **Specify Query Parameters**:
       - Add the parameters you want CloudFront to consider as part of the cache key.
       - Example: Include parameters like `?version=`, `?user=`, `?lang=` for different versions or language-based content.
   - **Save** the policy and assign it to the cache behavior.

5. **Save Cache Behavior Changes**:
   - After selecting or creating the cache policy, save the cache behavior settings.

---

## Testing and Verifying

1. **Deploy and Test**:
   - Allow some time for the distribution changes to propagate.
   - Test by accessing URLs with various query parameters to ensure CloudFront is caching and serving the correct variations.

2. **Check Cache Hit/Miss in CloudFront Logs**:
   - Use CloudFront access logs or monitoring tools to inspect cache hits and misses based on query parameters.
   - Look for cache keys in logs that include the specified query parameters.

3. **Inspect Headers**:
   - Use developer tools (e.g., Chrome DevTools) to check `x-cache` response headers:
     - `x-cache: Hit from cloudfront` indicates a cached response.
     - `x-cache: Miss from cloudfront` indicates a new cache entry was created.

---

## Example Configuration

Here’s an example of how query parameters could be configured in a custom cache policy:

- **Path Pattern**: `/content/*`
- **Query Parameters in Cache Key**: 
  - `version`: Cache variations of the content based on versioning (e.g., `?version=1` or `?version=2`).
  - `lang`: Cache different language content (e.g., `?lang=en` or `?lang=es`).
  
By configuring this, CloudFront will treat requests to `/content/page.html?version=1&lang=en` and `/content/page.html?version=2&lang=es` as separate cache keys.

---

## Conclusion

Including query parameters in the cache key is essential for applications serving dynamic content based on parameters, like user settings or content versions. Configuring cache keys effectively improves cache hit ratios, reduces origin load, and enhances user experience by serving content tailored to specific request parameters.

For more details, refer to the [AWS CloudFront Documentation on Cache Keys](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/working-with-cache-keys.html).

---


```
Go to CloudFront > edit behavior > cache policy > view policy > edit policy > set Cache key settings.