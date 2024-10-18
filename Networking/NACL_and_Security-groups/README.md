# AWS Network Access Control List (NACL) and Security Groups

## Overview
In AWS, **Network Access Control Lists (NACLs)** and **Security Groups** are crucial components for controlling the inbound and outbound traffic to your resources. They provide different levels of security, working together to protect your environment.

### Network Access Control List (NACL)
- **Level**: Operates at the subnet level.
- **Statefulness**: NACLs are stateless, meaning they evaluate each request independently.
- **Rule Processing**: Rules are evaluated in numerical order, and the first matching rule is applied.
- **Allow and Deny**: You can explicitly allow or deny traffic based on IP address, port, and protocol.

### Security Groups
- **Level**: Operates at the instance level.
- **Statefulness**: Security Groups are stateful, meaning if inbound traffic is allowed, the outbound traffic for that request is also allowed automatically.
- **Rule Processing**: All rules are evaluated and there is no specific order.
- **Allow Only**: You can only define rules to allow traffic; deny rules are not possible.

## Setting Up NACLs

1. Go to the **VPC Dashboard** in the AWS Management Console.
2. Select **Network ACLs** from the left sidebar.
3. Click on **Create Network ACL** and specify the required VPC.
4. Once created, go to the **Inbound Rules** and **Outbound Rules** tabs to add rules.
5. Define rules with specific conditions (e.g., source IP, destination port, protocol) and set them to **allow** or **deny**.
6. Associate the NACL with the desired subnets.

## Setting Up Security Groups

1. Go to the **EC2 Dashboard** in the AWS Management Console.
2. Click on **Security Groups** in the left sidebar.
3. Click on **Create Security Group** and fill in the details like name, description, and the VPC where the security group will reside.
4. Define **Inbound Rules**:
   - Specify the protocol (e.g., TCP, UDP), port range, and source IP or security group.
   - Example Rules:
     - Allow SSH access (port 22) from a specific IP address.
     - Allow HTTP access (port 80) from anywhere.
5. Define **Outbound Rules**:
   - Typically, all outbound traffic is allowed by default. Modify the rules if necessary.

## Key Differences Between NACLs and Security Groups

| Feature                  | NACLs                              | Security Groups                      |
|--------------------------|-----------------------------------|-------------------------------------|
| **Level**                | Subnet level                      | Instance level                      |
| **Statefulness**         | Stateless                         | Stateful                            |
| **Rule Processing**      | Rules processed in order          | All rules processed together        |
| **Allow/Deny**           | Can allow or deny traffic         | Can only allow traffic              |

## Best Practices
1. **Use NACLs for broad traffic control**: Apply NACLs to set policies for multiple resources within a subnet.
2. **Use Security Groups for specific resource control**: Use Security Groups to protect individual EC2 instances.
3. **Keep rules simple**: Avoid complex rules for easier management and troubleshooting.
4. **Restrict access as much as possible**: Only allow traffic that is essential for your resources to function.

## Conclusion
Using both NACLs and Security Groups provides a layered approach to securing your AWS environment. They offer flexibility in managing traffic at both the subnet level and the instance level, ensuring your resources are protected.
