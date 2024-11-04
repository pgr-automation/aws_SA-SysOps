# AWS ELB Monitoring, Tracing, and Logging

This README provides an overview of how to set up **Monitoring**, **Tracing**, and **Logging** for AWS Elastic Load Balancers (ELB), including Application Load Balancer (ALB), Network Load Balancer (NLB), and Classic Load Balancer (CLB). Effective monitoring and logging help maintain the health, performance, and security of your load-balanced applications.

---

## 1. Monitoring ELB

### Overview
AWS provides several monitoring options for ELB via **Amazon CloudWatch**, allowing you to monitor performance, error rates, traffic, and more. Monitoring helps you track load balancer health, adjust scaling, and diagnose issues.

### Key Metrics to Monitor
- **RequestCount**: Number of requests processed by the load balancer.
- **TargetResponseTime (ALB)**: Time taken for targets to respond.
- **HealthyHostCount / UnHealthyHostCount**: Number of healthy and unhealthy instances.
- **HTTPCode_ELB_4XX_Count / HTTPCode_ELB_5XX_Count (ALB)**: Counts of 4xx/5xx errors generated by the ELB.
- **ProcessedBytes**: Volume of incoming and outgoing data.

### How to Enable Monitoring
1. Go to **CloudWatch** in the AWS Management Console.
2. Choose **Metrics** > **ELB**.
3. Select your load balancer and configure alarms for specific metrics to receive notifications when thresholds are breached.

### Recommended Alarms
- **High Latency Alarm**: Set an alarm for `TargetResponseTime`.
- **Health Check Failures**: Set alarms on `UnHealthyHostCount`.
- **Error Rates**: Alarms for `HTTPCode_ELB_4XX_Count` and `HTTPCode_ELB_5XX_Count` for error rates.

---

## 2. Tracing ELB Requests

### Overview
AWS X-Ray helps trace and analyze requests as they travel through your load balancer to backend applications, enabling end-to-end visibility of application performance.

### How to Enable X-Ray with ELB
1. Ensure **AWS X-Ray** is enabled for your backend instances or container services.
2. Integrate X-Ray SDK with your application code to capture request traces.
3. View traces in the **AWS X-Ray console** to analyze latency, request errors, and bottlenecks in application components.

### Benefits of Tracing
- Identify high-latency requests or errors across distributed applications.
- Gain insights into end-user experience.
- Monitor performance of backend services in real-time.

---

## 3. Logging ELB Activity

### Access Logs
Access logs provide detailed information about requests sent to your load balancer, including the client’s IP, request type, and backend response time.

#### How to Enable Access Logs
1. Go to the **Load Balancer** in the AWS Management Console.
2. Under **Attributes**, enable **Access Logs**.
3. Specify an **S3 bucket** to store the logs and configure the log format as needed.

#### Access Log Contents
Each access log entry includes:
- **Timestamp**: Date and time of the request.
- **Client IP**: IP address of the client.
- **Target**: IP address and port of the backend instance.
- **Request Processing Time**: Time taken by ELB to process the request.
- **Target Processing Time**: Time taken by the target instance to respond.

### Request Tracing (ALB Specific)
Application Load Balancers support **Request Tracing** by adding a unique `X-Amzn-Trace-Id` header to each request. This ID can be tracked through logs and application layers for detailed tracing.

### CloudTrail Logs
Enable **AWS CloudTrail** to log API calls related to ELB configuration changes, including actions like creating, modifying, and deleting load balancers and their settings.

---

## Example Use Cases

- **Monitoring High Traffic**: Set up CloudWatch alarms to notify you when traffic spikes and adjust auto-scaling as needed.
- **Tracing Latency Issues**: Use AWS X-Ray to trace requests and identify which components contribute most to latency.
- **Analyzing Error Rates**: Monitor and log 4xx/5xx errors to troubleshoot client or server issues effectively.

---

## Notes
- **Log Retention**: Manage S3 bucket lifecycle policies to retain or delete logs as per your organization’s data policy.
- **Best Practices**: Use both monitoring and logging for comprehensive observability of your load balancer and backend performance.

---

## References
- [Amazon CloudWatch ELB Metrics](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/load-balancer-cloudwatch-metrics.html)
- [AWS X-Ray Tracing](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html)
- [ELB Access Logs](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html)
- [AWS CloudTrail for ELB](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/elb-cloudtrail-logs.html)

---
