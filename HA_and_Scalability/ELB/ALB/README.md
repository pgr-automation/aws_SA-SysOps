# AWS Application Load Balancer (ALB)

**AWS Application Load Balancer (ALB)** is a load balancing service that automatically distributes incoming HTTP and HTTPS traffic across multiple targets (like EC2 instances, containers, or IP addresses). ALB is optimized for application-level (Layer 7) load balancing and offers advanced features for routing and managing traffic to applications, microservices, and containers.

---

## Why Use ALB?
- **Application-Level Routing**: ALB allows for host-based and path-based routing, making it ideal for microservices and complex applications with multiple endpoints.
- **Enhanced Security and SSL Offloading**: ALB integrates with AWS Certificate Manager (ACM) for SSL/TLS certificates and handles encryption at the load balancer level, reducing backend workload.
- **WebSocket and HTTP/2 Support**: ALB supports WebSocket and HTTP/2 protocols, enabling high-performance, real-time applications.
- **Automatic Scaling**: ALB automatically scales to handle variable traffic loads, maintaining high availability and performance.
- **Integrated Health Checks**: ALB includes health checks to ensure only healthy targets receive traffic, enhancing application reliability.

---

## Key Features of ALB

1. **Path-Based and Host-Based Routing**
   - Route traffic based on URL paths (e.g., `/api`, `/login`) or host headers (e.g., `app.example.com`, `api.example.com`), allowing fine-grained control of traffic flow.

2. **SSL/TLS Termination**
   - Offloads SSL termination, allowing ALB to handle encryption/decryption. This frees up resources on backend instances and provides central management of SSL certificates.

3. **WebSocket and HTTP/2 Support**
   - Supports WebSocket for full-duplex communication and HTTP/2 for efficient multiplexing, making it suitable for real-time and low-latency applications.

4. **Containerized Application Support**
   - Integrates with Amazon ECS and Kubernetes (EKS) to handle multiple containers and microservices, automatically registering and deregistering targets as they scale.

5. **Sticky Sessions**
   - Ensures user sessions are routed to the same backend server, which is useful for applications that need session persistence (like e-commerce applications).

6. **Native Authentication Support**
   - ALB can authenticate users through Amazon Cognito or other OIDC-compliant identity providers, adding a layer of security for applications.

7. **Enhanced Logging and Monitoring**
   - Integrates with AWS CloudWatch, providing access logs for monitoring traffic patterns and gaining insights into request behaviors.

---

## Use Cases for AWS ALB

1. **Microservices and Containerized Applications**
   - **Description**: Microservices architectures, often deployed in containers, require flexible routing based on host or path.
   - **Solution**: ALB’s path-based and host-based routing allows you to direct traffic to specific microservices, making it an ideal choice for containerized applications running on ECS or EKS.

2. **Complex Web Applications**
   - **Description**: Web applications with multiple endpoints (e.g., `/products`, `/login`, `/api`) require routing to different backend services.
   - **Solution**: Use ALB to define routing rules based on URL paths, directing traffic to appropriate backend services based on the request.

3. **API Gateway**
   - **Description**: Many applications need a lightweight API gateway for managing and routing HTTP/HTTPS requests to backend services.
   - **Solution**: ALB can act as an API gateway, offering path-based routing to multiple endpoints, SSL termination, and WebSocket support for real-time API communication.

4. **Single Sign-On (SSO) and Authentication**
   - **Description**: Applications requiring user authentication before accessing services benefit from built-in authentication.
   - **Solution**: ALB integrates with Amazon Cognito and OIDC providers to manage user authentication, simplifying security for applications without needing to manage the authentication layer separately.

5. **WebSocket Applications**
   - **Description**: Real-time applications (e.g., chat applications, stock trading platforms) that require full-duplex communication benefit from WebSocket support.
   - **Solution**: ALB supports WebSocket and HTTP/2 protocols, allowing real-time data flow and full-duplex communication between clients and servers.

6. **Session-Based Applications (e.g., E-Commerce Sites)**
   - **Description**: Applications that maintain user sessions benefit from routing requests to the same backend instance.
   - **Solution**: ALB offers sticky sessions, directing user requests to the same backend server to maintain session continuity, ideal for e-commerce and other session-based applications.

---

## Summary

The AWS Application Load Balancer (ALB) provides advanced Layer 7 load balancing, making it ideal for applications with complex routing requirements, such as microservices, containerized applications, and real-time communication. With features like path-based and host-based routing, SSL termination, WebSocket support, and native user authentication, ALB enables high availability, security, and performance for modern web applications. 

Using ALB in your architecture can improve scalability, enhance user experience, and simplify management for complex web-based applications.


## ALB Creation 
```
Step: 1 Create a target group
1. Create SG and allow port 80 or 443 to access only from ALB
2. Create EC2 Instance 
3. Create a target group 
 	Configure below:
 	- choose trget type
 	- Give targrt group name
 	- Select protocal with required port (http/https/tcp/udp/tls etc...)
 	- Select VPC
 	- Selet Protocal Version (HTTP1/HTTP2)
 	- Configure Health check if required
4. Register the targets with created EC2 instances. and configure instance port 
5. Include as pending below
6. Create.

Step: 2 
1. Create Load Balancer
	- scheme -> Internet facinf
	- IP adress type -> IPV4
	- Network Mapping -> Select VPC -> selet availability zones
	- Selet security which is created for ALB
	
2. Listers and mapping 
	- select Protocal(http/https etc..) and target group
	
3. Create Load balancer , once created test the DNS name.
```


# AWS Application Load Balancer (ALB) Rules

**AWS Application Load Balancer (ALB)** supports advanced routing capabilities through the use of rules. These rules allow you to define how incoming requests are processed and routed to target groups based on specific conditions. This document provides an overview of ALB rules, their types, configuration, and examples.

---

## Overview of ALB Rules

ALB rules are evaluated in the order they are defined, with the first matching rule being applied to the incoming request. If a request does not match any rules, it is routed to the default action specified for the ALB.

### Types of ALB Rules

1. **Host-Based Routing**
   - Route traffic based on the hostname in the request (e.g., `example.com`, `api.example.com`).
   - Useful for directing traffic to different services or applications hosted on the same load balancer.

2. **Path-Based Routing**
   - Route traffic based on the URL path of the request (e.g., `/api/*`, `/images/*`).
   - Enables you to direct requests to different target groups based on specific paths.

3. **HTTP Header-Based Routing**
   - Route requests based on the values of HTTP headers (e.g., `User-Agent`, `X-Forwarded-For`).
   - Allows for more granular control of traffic based on client characteristics.

4. **Query String Parameter-Based Routing**
   - Route requests based on the values of query string parameters (e.g., `?version=1.0`).
   - Useful for directing traffic to different versions of an application.

5. **HTTP Method-Based Routing**
   - Route requests based on the HTTP method (e.g., `GET`, `POST`, `PUT`).
   - Useful for APIs that have different actions for different methods.

---

## Configuring ALB Rules

### Step 1: Navigate to the ALB in the AWS Management Console
1. Sign in to the [AWS Management Console](https://aws.amazon.com/console/).
2. Go to the EC2 Dashboard and click on **Load Balancers** under **Load Balancing**.
3. Select your Application Load Balancer.

### Step 2: Create or Modify a Listener
1. Under the **Listeners** tab, click on the **View/edit rules** option for the desired listener (usually HTTP or HTTPS).
2. To create a new rule, click **Add rule**.

### Step 3: Define Conditions for the Rule
1. **Add conditions**:
   - **Host header**: Specify the hostnames to match.
   - **Path**: Specify the URL paths to match.
   - **HTTP headers**: Add specific headers and their values.
   - **Query strings**: Specify query parameters and their values.
   - **HTTP methods**: Specify the HTTP methods to match.
2. **Select the action**:
   - **Forward**: Specify the target group to forward the request to.
   - **Redirect**: Redirect requests to a different URL.
   - **Return fixed responses**: Return a fixed response (e.g., a 404 page).

### Step 4: Review and Save the Rule
1. Review the conditions and action specified for the rule.
2. Click **Save** to apply the changes.

---

## Examples of ALB Rules

### Example 1: Host-Based Routing
- **Condition**: If the host is `api.example.com`
- **Action**: Forward to the `api-target-group`

### Example 2: Path-Based Routing
- **Condition**: If the path is `/images/*`
- **Action**: Forward to the `images-target-group`

### Example 3: Query String Parameter-Based Routing
- **Condition**: If the query string parameter `version` equals `1.0`
- **Action**: Forward to the `v1-target-group`

### Example 4: Redirect Rule
- **Condition**: If the path is `/old-path`
- **Action**: Redirect to `https://example.com/new-path`

### Example 5: Return Fixed Response
- **Condition**: If the path is `/maintenance`
- **Action**: Return a fixed response with a 503 status code and a custom message.

---

## Conclusion

Application Load Balancer rules provide powerful routing capabilities for managing and directing incoming traffic based on specific conditions. By using host-based, path-based, header-based, query string parameter-based, and HTTP method-based routing, you can enhance the flexibility and performance of your applications.

For more details and advanced configurations, refer to the [AWS ALB Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html).


# AWS Application Load Balancer (ALB) Rules

This document provides an overview of configuring rules for AWS Application Load Balancer (ALB). ALB rules define how incoming requests are routed to targets based on conditions and actions. This enables fine-grained control over request handling and improves load balancing performance.

---

## Rule Components

### 1. Conditions
Rules can be configured to route requests based on various conditions:

- **Host Header**: Route requests based on the domain name. 
  - **Example**: Route `www.example.com` traffic to a specific target group.
- **Path**: Route requests based on the URL path.
  - **Example**: Requests with path `/api/*` are routed to an API-specific target group.
- **HTTP Header**: Route requests based on the presence and value of a specific HTTP header.
  - **Example**: Route requests with `X-Header=Test` to a designated target group.
- **HTTP Method**: Route requests based on HTTP methods such as `GET`, `POST`, `PUT`, etc.
- **Query String**: Route requests based on key-value pairs in the URL query string.
  - **Example**: Route requests with `?user=admin` to an admin-specific target group.
- **Source IP**: Route requests based on the client's IP address.
  - **Example**: Allow access only to specific IP ranges.

### 2. Actions
Rules also define the actions to take once a condition is met:

- **Forward**: Forward requests to a specified target group.
- **Redirect**: Redirect requests to another URL or protocol (e.g., HTTP to HTTPS).
  - **Options**: Set status code (`HTTP 301` or `HTTP 302`) and redirection path.
- **Fixed Response**: Return a predefined response, such as an HTTP status code, headers, and a message body.
  - **Example**: Return a `403 Forbidden` response for unauthorized requests.
- **Authenticate**: Redirect requests to an authentication service like Amazon Cognito or OpenID Connect (OIDC) for user verification.

### 3. Priority
Each rule has a priority that determines the order in which rules are evaluated:

- **Lowest Priority**: Rules with lower numbers are evaluated first.
- **Default Rule**: The default rule (priority `0`) applies if no other rules match the request conditions.

---

## Example Rule Configuration

Below is a sample configuration for ALB rules in YAML format:

```yaml
Rules:
  - Priority: 1
    Conditions:
      - Field: host-header
        HostHeaderConfig:
          Values: ["www.example.com"]
      - Field: path-pattern
        PathPatternConfig:
          Values: ["/api/*"]
    Actions:
      - Type: forward
        TargetGroupArn: "arn:aws:elasticloadbalancing:region:account-id:targetgroup/example-target-group"

  - Priority: 2
    Conditions:
      - Field: http-header
        HttpHeaderConfig:
          HttpHeaderName: "X-Header"
          Values: ["Test"]
    Actions:
      - Type: redirect
        RedirectConfig:
          Protocol: HTTPS
          Port: "443"
          StatusCode: HTTP_301
          Host: "new.example.com"
          Path: "/new-path"

  - Priority: 3
    Conditions:
      - Field: source-ip
        SourceIpConfig:
          Values: ["192.168.0.0/24"]
    Actions:
      - Type: fixed-response
        FixedResponseConfig:
          StatusCode: "403"
          ContentType: "text/plain"
          MessageBody: "Access Denied"
```