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