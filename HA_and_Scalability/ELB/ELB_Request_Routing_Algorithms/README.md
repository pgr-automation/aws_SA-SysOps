# AWS Elastic Load Balancer (ELB) Request Routing Algorithms

This document provides an overview of the request routing algorithms available for AWS Elastic Load Balancer (ELB) types. Each ELB type offers specific routing algorithms that help distribute incoming requests across multiple targets to ensure high availability and optimal performance.

---

## ELB Types and Request Routing Algorithms

### 1. Application Load Balancer (ALB)
- **Routing Algorithms**:
  - **Round Robin**: Distributes incoming requests sequentially across healthy targets. This is the default algorithm for ALB.
  - **Least Outstanding Requests**: Routes requests to the target with the fewest active requests, ensuring load is evenly distributed.
- **Use Case**: Ideal for web applications, microservices, and container-based setups where requests need to be routed based on HTTP/HTTPS paths or host headers.

### 2. Network Load Balancer (NLB)
- **Routing Algorithms**:
  - **Flow Hash**: Routes connections based on a flow hash algorithm that considers protocol, source IP, source port, destination IP, destination port, and TCP sequence number.
  - **Source IP Hash**: Routes connections consistently based on the client’s source IP address, especially for UDP and TCP listeners.
- **Use Case**: Suitable for applications that require high throughput, low latency, and IP-based routing. Often used for handling large volumes of connections at layer 4.

### 3. Gateway Load Balancer (GWLB)
- **Routing Algorithm**:
  - **Flow Hash**: Uses a flow-hash algorithm to route traffic consistently for each session or flow.
- **Use Case**: Designed for network traffic inspection, and virtual appliances such as firewalls, intrusion detection/prevention systems (IDPS), and deep packet inspection (DPI).

### 4. Classic Load Balancer (CLB)
- **Routing Algorithms**:
  - **Round Robin** (for HTTP/HTTPS listeners): Routes each request sequentially across healthy instances.
  - **Least Connection** (for TCP listeners): Routes connections to the instance with the fewest active connections.
- **Use Case**: Primarily for legacy applications or basic load balancing needs. Operates at both layer 4 (TCP) and layer 7 (HTTP/HTTPS) but offers limited features compared to ALB and NLB.

---

## Summary Table of ELB Routing Algorithms

| Load Balancer Type       | Protocol Layer | Routing Algorithm              | Ideal Use Cases                                      |
|--------------------------|----------------|--------------------------------|------------------------------------------------------|
| **Application (ALB)**    | Layer 7        | Round Robin, Least Outstanding Requests | Web apps, microservices, host/path-based routing |
| **Network (NLB)**        | Layer 4        | Flow Hash, Source IP Hash      | High throughput, low latency, IP-based routing       |
| **Gateway (GWLB)**       | Layer 3/4      | Flow Hash                      | Traffic inspection, virtual network appliances       |
| **Classic (CLB)**        | Layer 4 & 7    | Round Robin, Least Connection  | Legacy applications, basic load balancing            |

---

## Notes
- **Application Load Balancer (ALB)**: Best for layer 7 traffic with support for host-based and path-based routing.
- **Network Load Balancer (NLB)**: Offers ultra-low latency and handles millions of requests per second, making it suitable for IP-based routing at layer 4.
- **Gateway Load Balancer (GWLB)**: Useful for deploying virtual appliances and traffic inspection across multiple Availability Zones.
- **Classic Load Balancer (CLB)**: AWS’s legacy load balancer, recommended only for simple applications or backward compatibility.

---

This document provides an overview of ELB routing algorithms to help select the best option for your application’s requirements.
