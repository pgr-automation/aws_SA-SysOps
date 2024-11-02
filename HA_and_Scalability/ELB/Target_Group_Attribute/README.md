# AWS Target Group Attributes

This document provides an overview of the key attributes available when configuring AWS Target Groups. Target Groups are used to route requests to one or more registered targets, such as EC2 instances, IP addresses, or Lambda functions.

## Attributes

### 1. Protocol
- **Description**: Defines the protocol used for routing traffic to targets.
- **Options**: `HTTP`, `HTTPS`, `TCP`
- **Notes**: Must be compatible with the Load Balancer's listener protocol.

### 2. Port
- **Description**: The port on which the target receives traffic.
- **Common Ports**: `80` (HTTP), `443` (HTTPS)

### 3. Target Type
- **Description**: Determines the type of targets within the group.
- **Options**:
  - `Instance`: Targets are EC2 instances.
  - `IP`: Targets are IP addresses.
  - `Lambda`: Targets are Lambda functions.

### 4. Health Check Settings
- **Health Check Protocol**: Protocol used for health checks. Options: `HTTP`, `HTTPS`, `TCP`.
- **Health Check Path**: Specific URL path for health checks (e.g., `/health`).
- **Health Check Interval**: Time between health checks in seconds.
- **Health Check Timeout**: Maximum time to wait for a health check response.
- **Healthy Threshold**: Number of successful health checks to mark a target as healthy.
- **Unhealthy Threshold**: Number of failed health checks to mark a target as unhealthy.

### 5. Deregistration Delay
- **Description**: Time (in seconds) that the load balancer waits before removing a target from rotation after deregistration.
- **Default**: `300` seconds

### 6. Stickiness
- **Description**: Enables sticky sessions, ensuring consistent routing for a client to the same target.
- **Attributes**:
  - **Stickiness Type**: Type of stickiness, such as `lb_cookie`.
  - **Duration**: Stickiness duration in seconds.

### 7. Load Balancer Affinity
- **Description**: Defines if the target group is tied to a specific load balancer.
- **Applies to**: Network Load Balancers

### 8. Slow Start Duration (Application Load Balancers)
- **Description**: Gradually increases traffic to a newly registered target, preventing it from being overwhelmed.
- **Duration**: Time in seconds

## Example Configuration

Here’s an example configuration for a target group in AWS:

```yaml
TargetGroup:
  Protocol: HTTP
  Port: 80
  TargetType: Instance
  HealthCheck:
    Protocol: HTTP
    Path: /health
    Interval: 30
    Timeout: 5
    HealthyThreshold: 3
    UnhealthyThreshold: 3
  Stickiness:
    Type: lb_cookie
    Duration: 300
  DeregistrationDelay: 300
  SlowStart: 60
```