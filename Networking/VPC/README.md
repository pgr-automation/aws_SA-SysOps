# AWS Virtual Private Cloud (VPC) Guide

## Table of Contents
1. [What is AWS VPC?](#what-is-aws-vpc)
2. [Core Components of AWS VPC](#core-components-of-aws-vpc)
3. [Common Real-time Issues with AWS VPC and Troubleshooting](#common-real-time-issues-with-aws-vpc-and-troubleshooting)
4. [Best Practices for AWS VPC Design](#best-practices-for-aws-vpc-design)
5. [Tools for Troubleshooting AWS VPC Issues](#tools-for-troubleshooting-aws-vpc-issues)
6. [Real-time Scenarios](#real-time-scenarios)

## What is AWS VPC?

**AWS VPC** (Virtual Private Cloud) is a service that lets you create a logically isolated network in the AWS cloud where you can launch AWS resources in a virtual environment. You have control over your virtual networking environment, including IP address ranges, subnets, route tables, and network gateways.

## Core Components of AWS VPC

1. **Subnets**:
   - **Public Subnets**: Subnets that can communicate with the internet through an Internet Gateway.
   - **Private Subnets**: Subnets that don't have direct access to the internet but can connect to it through a NAT Gateway/Instance.

2. **Route Tables**:
   - Determines how network traffic is directed. Each subnet in a VPC must be associated with a route table.

3. **Internet Gateway (IGW)**:
   - A gateway that allows communication between instances in your VPC and the internet.

4. **NAT Gateway/Instance**:
   - Allows instances in a private subnet to connect to the internet or other AWS services but prevents the internet from initiating a connection with those instances.

5. **Security Groups**:
   - Act as a virtual firewall for instances to control inbound and outbound traffic.

6. **Network Access Control Lists (NACLs)**:
   - Optional layers of security that control traffic to and from subnets.

7. **VPC Peering**:
   - A networking connection between two VPCs that enables traffic routing using private IP addresses.

8. **VPC Endpoints**:
   - Allows you to privately connect your VPC to supported AWS services without using an IGW, NAT device, VPN, or Direct Connect.

## Common Real-time Issues with AWS VPC and Troubleshooting

### 1. Instances in Private Subnet Cannot Access the Internet

- **Possible Cause**: No NAT Gateway/Instance is configured, or the NAT Gateway is in the wrong subnet.
- **Troubleshooting Steps**:
  - Ensure that a NAT Gateway/Instance is associated with the route table of the private subnet.
  - Verify that the NAT Gateway is in a public subnet with a route to the Internet Gateway.
  - Check the route table of the private subnet to ensure there is a default route (0.0.0.0/0) pointing to the NAT Gateway.

### 2. Unable to Access EC2 Instances in a Public Subnet

- **Possible Cause**: Security groups or NACLs are blocking traffic.
- **Troubleshooting Steps**:
  - Check the security group rules associated with the instance to make sure they allow inbound traffic on the required ports (e.g., SSH port 22 for Linux or RDP port 3389 for Windows).
  - Ensure the subnet's route table has a route to the Internet Gateway.
  - Verify the NACLs to make sure they are not blocking traffic to/from the instance.

### 3. VPC Peering Connection Issues

- **Possible Cause**: Route tables are not configured properly for the peering connection.
- **Troubleshooting Steps**:
  - Confirm that the route tables in both VPCs include routes for the peered VPCs' CIDR blocks.
  - Ensure that the security groups allow traffic from the peered VPC.
  - Check if overlapping IP ranges exist between the VPCs, which can cause routing conflicts.

### 4. High Latency or Packet Loss in VPC

- **Possible Cause**: Network bottlenecks or misconfigured settings.
- **Troubleshooting Steps**:
  - Check for high CPU or memory usage on the EC2 instances that might be causing delays.
  - Use **VPC Flow Logs** to analyze traffic patterns and detect any unusual spikes or network traffic.
  - Verify that there are no misconfigurations in security groups or NACLs that could be causing drops in traffic.

### 5. Instances in the Same VPC Cannot Communicate

- **Possible Cause**: Security group rules or NACLs are incorrectly set up.
- **Troubleshooting Steps**:
  - Check the security group attached to the instances to ensure they allow inbound and outbound traffic for the necessary ports and protocols.
  - Verify that NACLs are not restricting traffic between subnets.
  - Ensure that the route tables are correctly set up for inter-subnet communication.

### 6. VPC Endpoint Connection Failure

- **Possible Cause**: Route tables not configured correctly or endpoint policies blocking traffic.
- **Troubleshooting Steps**:
  - Check that the route table in the private subnet has a route to the VPC endpoint.
  - Ensure that the endpoint policy allows access to the required AWS service.
  - Verify security group settings to allow communication with the VPC endpoint.

## Best Practices for AWS VPC Design

1. **Segregate Public and Private Subnets**:
   - Use public subnets for resources that need direct internet access and private subnets for internal resources.

2. **Use Multiple Availability Zones (AZs)**:
   - Design your VPC with subnets spread across different AZs to increase redundancy and fault tolerance.

3. **Implement Security Controls**:
   - Use security groups and NACLs to protect resources at both the instance and subnet levels.
   - Regularly audit security group rules to remove unnecessary permissions.

4. **Monitoring and Logging**:
   - Enable **VPC Flow Logs** to capture IP traffic data for analysis and troubleshooting.
   - Use AWS CloudWatch to monitor the health of your VPC components and take action when issues arise.

5. **Use VPC Endpoints**:
   - Use VPC endpoints for private access to AWS services, reducing the need for an Internet Gateway.

6. **Plan CIDR Blocks Carefully**:
   - Avoid overlapping CIDR blocks in your VPCs, especially if you plan to implement VPC peering or connect with on-premises data centers.

## Tools for Troubleshooting AWS VPC Issues

- **VPC Flow Logs**:
  - Useful for monitoring network traffic and identifying connection issues or unauthorized access attempts.
  
- **AWS CloudWatch**:
  - Provides metrics and logging capabilities to monitor the health of VPC resources and set up alarms for unusual activities.

- **AWS Network Manager**:
  - Helps to visualize and troubleshoot your AWS network architecture, including VPCs and on-premises connections.

## Real-time Scenarios

### Scenario: Your application in a private subnet needs to access an S3 bucket but is experiencing connectivity issues.
- **Troubleshooting**:
  - Set up a VPC Endpoint for S3 to allow the application to access S3 without traversing the internet.
  - Ensure that your route table includes a route for the VPC Endpoint.

### Scenario: A new subnet is not communicating with your existing subnets in the same VPC.
- **Troubleshooting**:
  - Verify that the route tables for the subnets allow communication with each other.
  - Check if NACLs or security groups are restricting traffic between subnets.

## Steps to Create a VPC with All Core Components Using AWS CLI

### 1. Create a VPC
```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16

```
* Output: This command will return a VpcId which you will use in the following steps.
* Explanation: This creates a VPC with a CIDR block of 10.0.0.0/16.

### 2. Create Subnets
```bash
# Create a public subnet in AZ1
aws ec2 create-subnet --vpc-id <VpcId> --cidr-block 10.0.1.0/24 --availability-zone <AZ1>

# Create a private subnet in AZ1
aws ec2 create-subnet --vpc-id <VpcId> --cidr-block 10.0.2.0/24 --availability-zone <AZ1>

# Create a public subnet in AZ2
aws ec2 create-subnet --vpc-id <VpcId> --cidr-block 10.0.3.0/24 --availability-zone <AZ2>

# Create a private subnet in AZ2
aws ec2 create-subnet --vpc-id <VpcId> --cidr-block 10.0.4.0/24 --availability-zone <AZ2>
```
### 3. Create an Internet Gateway (IGW) and Attach it to the VPC
```bash
# Create an Internet Gateway
aws ec2 create-internet-gateway

# Attach the Internet Gateway to your VPC
aws ec2 attach-internet-gateway --vpc-id <VpcId> --internet-gateway-id <InternetGatewayId>
```

