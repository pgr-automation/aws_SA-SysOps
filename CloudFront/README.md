# AWS CloudFront

AWS CloudFront is a Content Delivery Network (CDN) service that securely delivers data, videos, applications, and APIs to users globally, with low latency and high transfer speeds. It caches content at edge locations worldwide, providing a fast, reliable way to distribute content.

---

## Key Features of AWS CloudFront

- **Global Content Distribution**: Distributes content through a global network of edge locations, reducing latency.
- **Security Integration**: Supports SSL/TLS encryption, AWS Shield, AWS Web Application Firewall (WAF), and field-level encryption for secure content delivery.
- **Dynamic and Static Content**: Delivers both static and dynamic content, including live streaming, on-demand video, web applications, and APIs.
- **Edge Computing**: Use AWS Lambda@Edge to run code closer to users, reducing latency for dynamic content and personalization.

---

## How AWS CloudFront Works

1. **Request Routing**: When a user requests content, CloudFront routes the request to the nearest edge location for fast delivery.
2. **Caching**: If the content is cached at the edge location, it’s delivered immediately. If not, CloudFront retrieves the content from the origin server (e.g., S3 bucket, EC2, or a custom origin).
3. **Content Distribution**: CloudFront caches the content at the edge location to serve future requests faster.

---

## Common Use Cases

- **Static Website Hosting**: Serving static websites from Amazon S3 with reduced latency.
- **APIs and Web Applications**: Distributing dynamic content, APIs, and web applications for global users.
- **Live and On-Demand Video Streaming**: Delivering media content with minimal latency and buffering.

---

## Setting Up AWS CloudFront

### 1. Create a CloudFront Distribution

1. Open the **CloudFront** service in the AWS Management Console.
2. Click **Create Distribution** and choose **Web**.
3. Specify the **Origin Domain**:
   - For S3: Choose your S3 bucket.
   - For custom origin: Enter the URL of your EC2 instance, ALB, or other custom source.
4. Configure **Cache and Behavior Settings**:
   - Set up path patterns, caching behavior, viewer protocol policy, and allowed HTTP methods.
5. Choose **Price Class** based on the geographic regions to target.
6. Configure **SSL Certificate** if HTTPS is required.
7. **Create the Distribution** and note the generated CloudFront domain name (e.g., `dxxxxxxx.cloudfront.net`).

### 2. Point Your Domain to CloudFront

- To use a custom domain, create a CNAME record in Route 53 (or your DNS provider) that points your domain (e.g., `www.example.com`) to the CloudFront distribution domain.

### 3. Configure HTTPS (Optional)

- CloudFront supports HTTPS by default. To use HTTPS with a custom domain, attach an SSL certificate via AWS Certificate Manager (ACM).

---

## Example Use: Serving Content from S3

Once you set up CloudFront with an S3 bucket as the origin:

1. **Upload files** to your S3 bucket.
2. **Access content** via the CloudFront distribution URL (`https://dxxxxxxx.cloudfront.net/your-file.jpg`), which will cache and serve the content globally.

---

## Example AWS CLI Commands for CloudFront

To create a CloudFront distribution with the AWS CLI:

```bash
aws cloudfront create-distribution --origin-domain-name <bucket_name>.s3.amazonaws.com
```