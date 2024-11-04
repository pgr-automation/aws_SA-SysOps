# Targrt Group
- a Target Group is a resource that directs requests to specific targets, such as EC2 instances, IP addresses, or AWS Lambda functions. It’s primarily used with load balancers like the Application Load Balancer (ALB) or Network Load Balancer (NLB) to distribute incoming traffic across multiple backend resources.


# Setting Up a Production-Grade AWS Target Group

## Prerequisites
- AWS Account with required permissions.
- A Virtual Private Cloud (VPC) configured with subnets across multiple Availability Zones (AZs).
- Security group configurations to allow necessary traffic (e.g., HTTP/HTTPS).

---

## Step 1: Define the Target Group

1. Navigate to the **EC2 Dashboard** in the AWS Console.
2. In the left sidebar, under **Load Balancing**, click **Target Groups**.
3. Click **Create target group**.
4. Configure the **Target group settings**:

   - **Target type**: Choose `Instance` for instance-based targets or `IP` for IP-based targets (useful for external/non-EC2 resources).
   - **Target group name**: Choose a meaningful name (e.g., `prod-app-tg`).
   - **Protocol**: Select `HTTPS` for secure traffic, or `HTTP` if your application does not use SSL.
   - **Port**: Enter the port on which your application listens (e.g., `443` for HTTPS).
   - **VPC**: Select the VPC where your instances are hosted.

5. **Advanced Health Checks** (important for production):

   - **Protocol**: Set to `HTTPS` for security, if your application supports it.
   - **Path**: Define a custom health check endpoint (e.g., `/health` or `/status`).
   - **Port**: Use `Traffic port` to check the target's default listening port.
   - **Healthy threshold**: Set to `5` for production to avoid false positives.
   - **Unhealthy threshold**: Set to `2` for quicker failure detection.
   - **Timeout**: Set to a reasonable time (e.g., `5` seconds) based on application response time.
   - **Interval**: Adjust as necessary for your traffic needs (e.g., `30` seconds for stability).
   - **Success codes**: Define expected status codes (e.g., `200` for HTTP 200 OK).

6. Click **Create target group**.

---

## Step 2: Add Targets to the Target Group

1. After creating the target group, you will see an option to **Register targets**.
2. Select the EC2 instances or IPs that you want to add to this target group.
   - For high availability, ensure that instances are distributed across multiple Availability Zones.
3. Click **Include as pending below** and then **Register pending targets**.

---

## Step 3: Attach Target Group to an Application Load Balancer (ALB)

1. In the **Load Balancers** section, click **Create Load Balancer** and select **Application Load Balancer**.
2. Configure the ALB:
   - **Name**: Provide a unique name (e.g., `prod-app-alb`).
   - **Scheme**: Choose `Internet-facing` for public access or `Internal` for private access.
   - **Availability Zones**: Select subnets across multiple AZs for high availability.
3. **Listeners and Routing**:
   - Add an **HTTPS listener** for production security.
   - In **Default actions**, select **Forward to** and choose the target group created earlier.

4. **Security Settings**:
   - If using HTTPS, upload an SSL certificate for secure communications.
   - Select or configure a security group to allow traffic on required ports (e.g., `443` for HTTPS).

5. Click **Create load balancer**.

---

## Step 4: Configure Auto Scaling (Optional but Recommended for Production)

1. Go to the **Auto Scaling Groups** section in EC2.
2. Create or modify an Auto Scaling Group to use the target group created above.
   - **Health Checks**: Set to use both **EC2** and **ELB** health checks for thorough monitoring.
   - **Scaling Policies**: Configure scaling based on metrics like CPU utilization or request count to adjust capacity as demand changes.

---

## Step 5: Monitor and Adjust

1. **CloudWatch Alarms**: Set up alarms on target group metrics such as `UnhealthyHostCount` and `RequestCount` for proactive monitoring.
2. **Logging**: Enable access logging for the load balancer to capture request details.
3. **Alerts**: Set up notifications for health and scaling events to quickly respond to issues.

---

By following these steps, you can establish a robust, secure, and highly available Target Group configuration, suitable for production environments.



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
6. Create
```