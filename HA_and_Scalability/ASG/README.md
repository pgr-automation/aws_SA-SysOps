# AWS Auto Scaling Groups (ASG)

## Overview
AWS Auto Scaling Groups (ASG) enable automated scaling of Amazon EC2 instances, allowing your infrastructure to automatically adjust to changes in demand. ASG helps ensure high availability, fault tolerance, and cost optimization by scaling out (adding instances) or scaling in (removing instances) as needed.

## Key Concepts

### 1. Launch Template or Launch Configuration
A **Launch Template** or **Launch Configuration** defines the specifications for the EC2 instances in an ASG. This includes the AMI ID, instance type, key pair, security groups, and additional configurations like IAM roles and user data scripts.

- **Launch Template**: Supports advanced features (e.g., multiple instance types, multiple network interfaces).
- **Launch Configuration**: An older version with limited features, but still functional.

### 2. Desired Capacity
The **Desired Capacity** is the target number of instances the ASG aims to maintain. This can be adjusted automatically by scaling policies or manually as needed.

### 3. Minimum and Maximum Capacity
These settings define the scaling boundaries of the ASG:
- **Minimum Capacity**: The minimum number of instances that must be running within the ASG.
- **Maximum Capacity**: The maximum number of instances that can run in the ASG.

### 4. Scaling Policies
**Scaling Policies** determine when and how the ASG scales in or out. There are several types:
- **Target Tracking Scaling**: Automatically adjusts the number of instances based on a target metric (e.g., maintaining 50% CPU utilization).
- **Step Scaling**: Adds or removes instances based on defined steps (e.g., adding 2 instances if CPU exceeds 70%).
- **Scheduled Scaling**: Scales instances based on a schedule, such as increasing capacity during peak hours.

### 5. Health Checks
**Health Checks** are used by ASG to determine the status of instances:
- **EC2 Health Check**: AWS automatically checks if the instance is running.
- **ELB Health Check**: If the ASG is associated with an Elastic Load Balancer (ELB), it can use ELB health checks to ensure instances are healthy in the load balancer's perspective.

### 6. Load Balancing
ASG can integrate with **Elastic Load Balancers (ELB)**, distributing incoming traffic across healthy instances. This ensures that ASG instances are efficiently utilized and enhances availability by spreading the load.

### 7. Lifecycle Hooks
**Lifecycle Hooks** allow custom actions to be performed at specific points in the ASG lifecycle. Hooks can be added for two events:
- **Instance Launch**: Trigger actions (like configuring an instance) before it is added to the group.
- **Instance Terminate**: Perform cleanup actions before the instance is removed from the group.

### 8. Instance Termination Policies
When ASG scales in, it needs to choose which instances to terminate. AWS provides termination policies to control this:
- **Default Policy**: Terminates instances based on criteria like age, AZ distribution, or cost.
- **Custom Policies**: Allow greater control over which instances are removed.

### 9. Cooldown Period
The **Cooldown Period** is a setting that defines a waiting period before ASG performs another scaling action, helping to prevent rapid scaling in response to short-term spikes in demand.

### 10. Monitoring and Metrics
ASG integrates with **Amazon CloudWatch** to monitor metrics and provide insights into the performance of your instances. Key metrics include CPU utilization, network traffic, and the number of instances in each scaling group.

### 11. Predictive Scaling
**Predictive Scaling** uses machine learning to anticipate changes in demand and adjusts ASG capacity ahead of time. This feature is useful for applications with predictable usage patterns.

### 12. Mixed Instances Policy
A **Mixed Instances Policy** allows the ASG to use multiple instance types and purchase options (e.g., Spot Instances and On-Demand Instances), providing cost optimization while ensuring capacity is met.

### 13. Capacity Rebalancing
**Capacity Rebalancing** helps maintain capacity when using Spot Instances by replacing instances if there is a risk of interruption. This feature helps ensure applications remain highly available even when using cost-effective Spot Instances.

## Example ASG Configuration

```yaml
ASG:
  MinSize: 2
  MaxSize: 10
  DesiredCapacity: 5
  LaunchTemplate: 
    LaunchTemplateId: "lt-0abcd1234efgh5678"
    Version: "1"
  HealthCheckType: "ELB"
  TargetGroupARNs:
    - "arn:aws:elasticloadbalancing:region:account-id:targetgroup/example-target-group/50dc6c495c0c9188"
  ScalingPolicies:
    - PolicyType: "TargetTrackingScaling"
      TargetValue: 50
      MetricType: "ASGAverageCPUUtilization"
    - PolicyType: "StepScaling"
      StepAdjustments:
        - MetricIntervalLowerBound: 0
          ScalingAdjustment: 2
```

## Best Practices

1. **Use Target Tracking for Stable Performance**  
   Target tracking scaling policies automatically adjust the number of instances based on a target metric, such as CPU utilization. This provides stable and predictable performance for your applications.

2. **Enable ELB Health Checks**  
   Use Elastic Load Balancer (ELB) health checks to monitor the status of instances in the ASG. This ensures that only healthy instances are active, enhancing the reliability of your application.

3. **Utilize Multiple Availability Zones (AZs)**  
   Configure the ASG to deploy instances across multiple Availability Zones. This increases fault tolerance by distributing instances, reducing the risk of outages from single AZ failures.

4. **Configure Lifecycle Hooks for Custom Tasks**  
   Use lifecycle hooks to perform custom actions, such as running configuration scripts, during instance launch or termination. This helps in setting up or cleaning up resources efficiently.

5. **Optimize with Mixed Instances Policy**  
   Leverage the Mixed Instances Policy to use a combination of On-Demand and Spot Instances. This reduces costs while ensuring your desired capacity is met by taking advantage of more economical instance types and pricing options.

6. **Monitor ASG Performance Using CloudWatch**  
   Integrate ASG with Amazon CloudWatch to monitor key metrics like CPU utilization, network traffic, and instance count. Regular monitoring helps fine-tune scaling policies based on real-time usage patterns and resource demands.

---

Following these best practices can enhance the reliability, performance, and cost-effectiveness of your AWS Auto Scaling Groups.



# Setting up AWS Auto Scaling Group (ASG) with Target Group and Elastic Load Balancer (ELB)

## Prerequisites
- AWS Account with necessary permissions.
- Amazon Virtual Private Cloud (VPC) with at least two subnets in different Availability Zones.

---

## Step 1: Create a Target Group

1. Go to the **EC2 Dashboard** in the AWS Management Console.
2. In the left menu, under **Load Balancing**, click on **Target Groups**.
3. Click **Create target group**.
4. Set the **Target type** to `Instance`.
5. Configure the **Target group details**:
   - **Target group name**: Give a meaningful name (e.g., `my-app-tg`).
   - **Protocol**: Choose `HTTP` or `HTTPS` depending on your app.
   - **Port**: Set the port your application uses (e.g., `80` for HTTP).
   - **VPC**: Select the VPC where your instances will run.
6. Configure **Health checks** (default settings work, but you can customize):
   - **Protocol**: Choose `HTTP` or `HTTPS`.
   - **Path**: Set a path (e.g., `/health`) for checking instance health.
7. Click **Create target group**.

---

## Step 2: Create an Application Load Balancer (ALB)

1. In the EC2 Dashboard, under **Load Balancing**, click **Load Balancers**.
2. Click **Create Load Balancer** and select **Application Load Balancer**.
3. Configure **Load Balancer settings**:
   - **Name**: Provide a unique name (e.g., `my-app-alb`).
   - **Scheme**: Choose `Internet-facing` for public access or `Internal` for private access.
   - **IP address type**: Choose `IPv4`.
4. Under **Network mapping**, select the VPC and subnets (at least two in different AZs).
5. Under **Security groups**, select an existing security group or create a new one allowing inbound traffic on the port your app uses (e.g., `80` for HTTP).
6. Under **Listeners and routing**:
   - **Listener**: Configure `HTTP` or `HTTPS` listener.
   - **Default action**: Select the target group you created in Step 1.
7. (Optional) If using HTTPS, upload your SSL certificate.
8. Click **Create load balancer**.

---

## Step 3: Create a Launch Template

1. In the **EC2 Dashboard**, click on **Launch Templates** under **Instances**.
2. Click **Create launch template**.
3. Provide **Launch template name** and **Description**.
4. Under **Application and OS Images (Amazon Machine Image)**, select an AMI.
5. Select an **Instance type** (e.g., `t2.micro`).
6. Configure **Key pair (login)** if SSH access is needed.
7. Configure **Network settings**:
   - Set **Security groups** to allow the required traffic (e.g., HTTP, HTTPS).
8. Add **Storage** settings if needed.
9. (Optional) Add **Advanced details** such as User Data to configure instances on launch.
10. Click **Create launch template**.

---

## Step 4: Create an Auto Scaling Group (ASG)

1. Go to the **Auto Scaling Groups** section in the EC2 Dashboard.
2. Click **Create Auto Scaling group**.
3. Name your ASG (e.g., `my-app-asg`) and choose the **Launch template** created in Step 3.
4. Click **Next** to configure **Instance purchase options**.
   - Select `On-Demand` or **Mixed instances** for cost optimization.
5. Under **Network**, select your **VPC** and **Subnets** (select at least two subnets across different AZs).
6. Attach the ASG to your Load Balancer:
   - **Attach to a load balancer**: Select `Application Load Balancer` or `Network Load Balancer`.
   - **Target Group**: Select the target group created in Step 1.
7. Configure **Health checks** to `ELB` for accurate health monitoring.
8. Configure the **Desired capacity**, **Minimum capacity**, and **Maximum capacity**.
9. Set up **Scaling policies**:
   - Choose **Target tracking scaling policy**.
   - **Metric type**: Select a metric (e.g., `ASGAverageCPUUtilization`).
   - **Target value**: Set a target value (e.g., `50` for 50% CPU utilization).
10. Configure **Notifications** (optional) to send alerts on scaling events.
11. Click **Create Auto Scaling group**.

---

## Step 5: Testing the Auto Scaling Group

1. Go to the **Load Balancers** section and copy the DNS name of your ALB.
2. Paste the DNS name in your browser. You should see your application if the instances are healthy and the scaling group is functioning correctly.
3. To test scaling, simulate high load (or use AWS CloudWatch Alarms) to verify that ASG adds instances according to your scaling policy.

---

## Cleanup (Optional)
To avoid unnecessary charges, delete the ASG, Target Group, ALB, and other resources created after testing.

---

This setup will automatically scale your instances up or down based on demand, distributing traffic across instances using the Application Load Balancer.
