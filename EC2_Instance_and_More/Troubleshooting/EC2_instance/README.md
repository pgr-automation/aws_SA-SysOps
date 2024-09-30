# EC2 Instance Issues and Troubleshooting

## 1. Instance Fails to Start

### Symptoms:
- Instance stuck in the "pending" or "stopping" state.
- Instance fails to initialize or boot.

### Troubleshooting Steps:
- **Instance Status Checks**: Navigate to the **EC2 Console** → **Instances** → **Status Checks** to identify if there are any failed instance checks.
- **Check System Logs**: Access the instance system logs from the **EC2 Console** under **Actions** → **Instance Settings** → **Get System Log**. Look for boot errors or misconfigurations.
- **Review Auto Scaling or Spot Requests**: If your instance was launched as part of an Auto Scaling group or Spot instance request, ensure there are sufficient resources available (e.g., not hitting limits for availability zones or instance types).

## 2. SSH Connection Issues

### Symptoms:
- Unable to connect via SSH.
- Timeout or `Connection refused` errors when trying to connect.

### Troubleshooting Steps:
- **Check Security Group Rules**: Ensure port 22 (or the custom SSH port) is open for your IP address in the security group attached to the instance.
- **Check Network ACLs**: Verify that the Network Access Control Lists (NACLs) associated with your VPC subnets are not blocking SSH traffic.
- **Verify Public IP**: Ensure you are using the correct public IP or Elastic IP to connect. If the instance is in a private subnet, you may need to connect via a bastion host.
- **Key Pair Issues**: Verify you are using the correct private key file and key pair specified during instance launch.
- **Reboot the Instance**: If connection issues persist, attempt to reboot the instance using the AWS Console or CLI.

## 3. High CPU Utilization

### Symptoms:
- EC2 instance has high CPU usage, leading to slow performance or unresponsiveness.

### Troubleshooting Steps:
- **Monitor CPU Utilization**: Use **CloudWatch** to monitor CPU metrics. Identify processes using high CPU by logging into the instance and using commands like `top` or `htop` for Linux, or the **Task Manager** for Windows.
- **Upgrade Instance Type**: If high CPU usage is due to a consistently high workload, consider upgrading to a larger instance type that meets your performance needs.
- **Auto Scaling**: For fluctuating CPU usage, consider setting up an **Auto Scaling group** to dynamically add more instances based on CPU thresholds.

## 4. Disk Space Issues

### Symptoms:
- Low or no available disk space on the EC2 instance, causing applications to crash or fail to start.

### Troubleshooting Steps:
- **Check Disk Usage**: Log into the instance and use `df -h` (Linux) or **Disk Management** (Windows) to check available disk space.
- **Clean Up Disk**: Remove unnecessary files, logs, or caches to free up space. For Linux, commands like `du -sh *` can help identify large directories.
- **Extend EBS Volume**: If you need more space, you can increase the size of the attached EBS volume using the AWS Console or CLI. After resizing, you may need to extend the filesystem on the instance.
    - Example for Linux:
    ```bash
    sudo growpart /dev/xvda 1
    sudo resize2fs /dev/xvda1
    ```

## 5. Instance Fails Health Checks

### Symptoms:
- Instance health checks fail consistently, indicating possible instance or host issues.

### Troubleshooting Steps:
- **System Status Checks**: Check if the failure is at the instance level (caused by software or configuration) or at the system level (caused by AWS hardware or network issues).
- **Reboot the Instance**: A simple reboot can sometimes resolve hardware or software issues.
- **Check Instance Logs**: Review system logs to see if there are any software errors that are causing the instance to fail checks. Use **CloudWatch Logs** if enabled, or fetch logs from the **EC2 Console**.
- **Stop and Start**: If the issue persists, try stopping and starting the instance (note that this will change the public IP unless you are using an Elastic IP).

## 6. Elastic IP Not Working

### Symptoms:
- Unable to reach the instance using the Elastic IP (EIP).

### Troubleshooting Steps:
- **Associate the EIP**: Make sure the Elastic IP is associated with the correct EC2 instance.
- **Check Security Groups**: Verify that the security group allows inbound traffic for the necessary ports.
- **Check Route Tables**: Ensure that the VPC route table for the instance’s subnet has a route to the internet via an Internet Gateway.

## 7. Instance Terminated Unexpectedly

### Symptoms:
- The EC2 instance is terminated without manual intervention.

### Troubleshooting Steps:
- **Spot Instances**: Spot instances can be terminated if the spot price exceeds your bid or if capacity is no longer available. Check the **Spot Instance Request** for termination reasons.
- **Auto Scaling Policies**: If part of an Auto Scaling group, ensure scaling policies do not trigger unexpected terminations.
- **Instance Retirement**: AWS may occasionally retire instances due to hardware issues. Check the AWS Console or notifications for retirement notices.

## 8. No Public IP Assigned to Instance

### Symptoms:
- Instance does not have a public IP address, making it unreachable from the internet.

### Troubleshooting Steps:
- **Subnet Settings**: Ensure the instance is launched in a subnet that has auto-assign public IP enabled.
- **Attach an Elastic IP**: If the instance is in a private subnet or does not have a public IP, allocate and attach an Elastic IP to it.
- **NAT Gateway Setup**: For private subnets, ensure proper routing through a NAT gateway for internet access.

## 9. Application Crashes on EC2

### Symptoms:
- An application running on the EC2 instance crashes or stops responding.

### Troubleshooting Steps:
- **Check Application Logs**: Review the logs to understand the cause of the crash.
- **Resource Constraints**: Monitor instance resources like CPU, memory, and disk space to identify potential bottlenecks.
- **Upgrade Instance Type**: If the crash is due to resource limits, consider upgrading the instance type to one with more CPU, memory, or storage.
