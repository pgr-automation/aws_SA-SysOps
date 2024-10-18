# AWS VPC Peering Setup Guide

## Overview
VPC (Virtual Private Cloud) peering in AWS allows you to connect two VPCs to communicate using private IP addresses as if they were on the same network. This guide provides a step-by-step process for creating and configuring VPC peering in AWS.

## Prerequisites
- Two VPCs with **non-overlapping CIDR ranges**.
- AWS credentials with permissions to manage VPCs.
- Access to the AWS Management Console or AWS CLI.

## Steps to Set Up VPC Peering

### 1. Create the VPC Peering Connection
1. Open the **Amazon VPC** console.
2. In the navigation pane, select **Peering Connections**.
3. Click on **Create Peering Connection**.
4. Specify the following details:
   - **Requester VPC**: The VPC you own.
   - **Accepter VPC**: The VPC you want to peer with (can be in the same or different AWS account).
5. Click on **Create Peering Connection**.

### 2. Accept the Peering Connection
- If the accepter VPC is in a different AWS account, log in to that account.
- Go to the **Peering Connections** section in the VPC console.
- Select the pending peering request and click **Accept Request**.

### 3. Update Route Tables
To enable communication between the VPCs, you need to update the route tables:
1. Open the **Route Tables** section in the VPC console.
2. Select the route table associated with your subnets.
3. Click on the **Routes** tab and add a new route:
   - **Destination**: CIDR block of the peered VPC.
   - **Target**: The VPC peering connection ID.
4. Repeat the process for the route table of the other VPC.

### 4. Modify Security Groups
Modify the security group rules to allow traffic between the two VPCs:
1. Open the **Security Groups** section in the VPC console.
2. Edit the security group rules to allow the desired traffic (e.g., HTTP or SSH) from the CIDR block of the peered VPC.
3. Save the changes.

### 5. Verify the Peering Connection
- Launch EC2 instances in both VPCs.
- Test the connection by pinging the private IP address of the instances in the peered VPC.
- Ensure that the security groups, network ACLs, and route tables are properly configured.

## Setting Up VPC Peering Using AWS CLI

You can also set up VPC peering using the AWS CLI:

1. **Create the Peering Connection**:
```bash
   aws ec2 create-vpc-peering-connection --vpc-id <VPC-ID-1> --peer-vpc-id <VPC-ID-2> --peer-region <region> --peer-owner-id <AWS-Account-ID>
```
2. **Accept the Peering Request**:
``` bash
   aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id <peering-connection-id>
```
3. **Update Route Tables**:
```bash
aws ec2 create-route --route-table-id <Route-Table-ID> --destination-cidr-block <CIDR-of-peered-VPC> --vpc-peering-connection-id <peering-connection-id>
```
4. **Modify Security Groups**:
    * Update the security groups to allow traffic from the CIDR block of the peered VPC.