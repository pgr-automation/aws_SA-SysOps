# AWS CloudFront Geo Restrictions

**Geo restrictions** in AWS CloudFront allow you to control access to your content based on the geographic location of your users. This can help in scenarios where you want to comply with legal regulations, restrict content to specific regions, or control access to licensed content.

---

## Benefits of Geo Restrictions

- **Content Control**: Limit access to specific countries or regions based on licensing agreements or legal restrictions.
- **Security**: Protect your content from unauthorized access in certain areas.
- **Cost Management**: Reduce traffic and bandwidth costs by limiting access to targeted geographic locations.

---

## Setting Up Geo Restrictions in CloudFront

### Step 1: Access Your CloudFront Distribution

1. Go to the **AWS CloudFront Console**.
2. Select the distribution where you want to configure geo restrictions.

### Step 2: Configure Geo Restrictions

1. Under **Settings**, go to **Geo Restriction**.
2. Select **Edit** to enable geo restrictions.
3. Choose one of the following options:
   - **Whitelist**: Specify the countries where content is **allowed**. Users in these countries will have access to your content, while others will be denied.
   - **Blacklist**: Specify the countries where content is **denied**. Users in these countries will not be able to access your content.

4. Enter the **Country Codes** for the countries you want to allow or block. (e.g., `US` for the United States, `CA` for Canada, `IN` for India).
5. Click **Save Changes**.

### Example Scenarios

- **Whitelist Use Case**: Restrict a video streaming service to operate only in the US and Canada.
- **Blacklist Use Case**: Block access from countries where your content should not be accessible due to legal or compliance requirements.

---

## Geo Restriction Using AWS WAF (Advanced)

For more advanced filtering based on geography, you can use **AWS WAF (Web Application Firewall)** with CloudFront:

1. Open the **AWS WAF Console**.
2. Create a **Web ACL** and associate it with your CloudFront distribution.
3. In the Web ACL settings, add a **Geo Match Condition**:
   - Select the countries you want to allow or block.
   - Define a custom rule to either allow or deny requests based on the geo match.
4. Attach the Web ACL to your CloudFront distribution.

---

## Testing Geo Restrictions

To test geo restrictions, use a VPN or a geo-testing tool to simulate access from different countries. You should see access denied messages for restricted regions and normal access for allowed regions.

---

## Important Notes

- **Accuracy**: CloudFront uses IP-based location data, which is usually accurate but not 100% reliable due to factors like VPNs and IP masking.
- **Cache Behavior**: Geo restrictions apply to all cache behaviors within a distribution.
- **AWS WAF Cost**: If you need highly customizable geo-blocking beyond standard geo restrictions, AWS WAF offers additional filtering at an extra cost.

---

## Additional Resources

- [AWS CloudFront Documentation](https://docs.aws.amazon.com/cloudfront/)
- [Geo Restriction Configuration Guide](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/georestrictions.html)
- [AWS WAF Documentation](https://docs.aws.amazon.com/waf/)

---

**Note**: Geo restrictions are enforced at CloudFront’s edge locations and may take a few minutes to propagate globally after changes.
