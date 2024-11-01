# AWS ELB Connection Draining and Deregistration Delay

This README provides an overview of **Connection Draining** for AWS Classic Load Balancers (CLB) and **Deregistration Delay** for Application Load Balancers (ALB) and Network Load Balancers (NLB) in AWS. These settings help manage connections gracefully when an instance is removed from the load balancer.

---

## 1. Connection Draining (Classic Load Balancer - CLB)

### Purpose
Connection draining ensures that active requests to an instance complete before the instance is deregistered or terminated, minimizing disruptions for users.

### How It Works
- When an instance is marked for deregistration or termination, the load balancer allows active connections to complete.
- No new connections are sent to the instance once it enters draining mode.
- The draining period is defined by a timeout value, after which any open connections are forcibly closed.

### Configuration Settings
- **Timeout Range**: 1 - 3600 seconds (1 hour)
- **Default Timeout**: 300 seconds

### Steps to Configure
1. Navigate to the **Classic Load Balancer** in the AWS Management Console.
2. Select **Attributes**.
3. Enable **Connection Draining** and set the desired **Timeout**.

---

## 2. Deregistration Delay (Application Load Balancer - ALB & Network Load Balancer - NLB)

### Purpose
Deregistration delay in ALB and NLB helps manage active connections by waiting a set amount of time before removing the instance from service.

### How It Works
- When an instance is marked for deregistration, ALB/NLB will wait for active connections to complete before ending them.
- During this delay period, no new connections are sent to the instance.
- This delay ensures that requests in progress complete successfully, reducing the chance of dropped requests.

### Configuration Settings
- **Timeout Range**: 1 - 3600 seconds
- **Default Timeout**: 300 seconds (recommended for HTTP/HTTPS traffic)

### Steps to Configure
1. Navigate to the **Target Group** associated with the ALB or NLB in the AWS Management Console.
2. Under **Attributes**, set the **Deregistration Delay** to the desired timeout value.

---

## Example Use Case
When scaling down instances behind a load balancer, setting a deregistration delay or enabling connection draining ensures that in-flight requests complete without interruption, providing a smooth experience for end users.

---

## Notes
- **Best Practices**: Adjust the timeout based on your application's average request duration. High-traffic or long-lived connections may benefit from a longer timeout, while short-lived connections can use a shorter delay.
- **Limitations**: If the timeout period is exceeded, active connections may be forcibly closed. Ensure that your application can handle such scenarios gracefully.

---

## References
For more information, refer to:
- [AWS Documentation on Connection Draining for CLB](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/config-conn-drain.html)
- [AWS Documentation on Deregistration Delay for ALB/NLB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html#deregistration-delay)

---

