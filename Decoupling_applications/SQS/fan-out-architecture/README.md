# AWS Fan-Out Architecture using SNS, SQS, Lambda, and Python

This README describes a fan-out architecture in AWS using SNS, SQS, and Lambda, with Python code to process messages. This design pattern enables the distribution of messages to multiple endpoints efficiently, ensuring that multiple services can independently consume the same message.

## Architecture Overview

- **Amazon SNS (Simple Notification Service)**: Acts as the message broker that distributes messages to subscribers (SQS queues and Lambda functions).
- **Amazon SQS (Simple Queue Service)**: Serves as message queues to buffer and store messages for different consumers.
- **AWS Lambda**: Processes the messages asynchronously and executes business logic using Python code.
- **Python code**: Used in the Lambda functions to process incoming messages.

## Steps to Implement Fan-Out Architecture

Follow these steps to set up the architecture in AWS.

### Step 1: Create an SNS Topic

1. Open the [Amazon SNS console](https://console.aws.amazon.com/sns).
2. Click on **Topics** in the left-hand menu.
3. Click **Create topic**.
4. Choose **Standard** as the topic type and provide a **Name**.
5. Click **Create topic**.

### Step 2: Create SQS Queues

Create multiple SQS queues that will be subscribed to the SNS topic.

1. Open the [Amazon SQS console](https://console.aws.amazon.com/sqs).
2. Click **Create queue**.
3. Choose **Standard** as the queue type.
4. Provide a **Name** for your queue (e.g., `QueueA`, `QueueB`).
5. Configure the required settings for the queue.
6. Click **Create queue**.
7. Repeat these steps to create additional queues as needed.

### Step 3: Subscribe SQS Queues to the SNS Topic

1. Open the [Amazon SNS console](https://console.aws.amazon.com/sns).
2. Click on the SNS topic you created.
3. In the **Subscriptions** tab, click **Create subscription**.
4. Set the **Protocol** to **SQS**.
5. Enter the ARN of the SQS queue created earlier in the **Endpoint** field.
6. Click **Create subscription**.
7. Repeat this step for each SQS queue you created.

### Create IAM roles for Lambda with 
1. SQS
2. CloudWatchLogs

### Step 4: Create a Lambda Function with Python Code

1. Open the [AWS Lambda console](https://console.aws.amazon.com/lambda).
2. Click **Create function**.
3. Choose **Author from scratch**.
4. Enter a **Function name** and select **Python 3.x** as the runtime.
5. Choose or create an **Execution role** with permissions to access SNS and SQS.
6. Click **Create function**.

Add the following example Python code to process messages:
```python
import json

def lambda_handler(event, context):
    # Log the event received from SNS
    print("Received event: " + json.dumps(event, indent=2))

    # Process each message
    for record in event['Records']:
        sns_message = record['Sns']['Message']
        print(f"Processing message: {sns_message}")

        # Your business logic here
        # For example, send an alert, update a database, etc.

    return {
        'statusCode': 200,
        'body': json.dumps('Message processed successfully')
    }
```
```python
import json

def lambda_handler(event, context):
    print(event['Records'][0]['body'])
    print("Event for Team A")
    return even
```

## Step 5: Subscribe the Lambda Function to the SNS Topic

1. Open the SNS console.
2. Select the SNS topic you created.
3. In the **Subscriptions** tab, click **Create subscription**.
4. Set the **Protocol** to **Lambda**.
5. Select the ARN of the Lambda function created earlier.
6. Click **Create subscription**.

### Step 6: Configure the SQS Queues with the Required Policy

To allow SNS to send messages to SQS, you must set up the appropriate policy for each SQS queue.

1. Open the Amazon SQS console.
2. Click on one of the queues.
3. Click on the **Permissions** tab and edit the **Access Policy**.
4. Add the following JSON policy:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": "*",
         "Action": "sqs:SendMessage",
         "Resource": "arn:aws:sqs:<region>:<account-id>:<queue-name>",
         "Condition": {
           "ArnEquals": {
             "aws:SourceArn": "arn:aws:sns:<region>:<account-id>:<topic-name>"
           }
         }
       }
     ]
   }
```
```
### step 7 :
1. Open the Amazon SNS console.
2. Select the SNS topic you created.
3. Click Publish message.
4. Enter a Message in the required field.
5. Click Publish message.

### How the Fan-Out Architecture Works

    SNS will distribute the message to all the SQS queues and Lambda functions that are subscribed to the topic.
    The SQS queues will store the messages until they are processed by consumers.
    The Lambda function will immediately receive the message and execute the processing logic using the Python code.


# Fan-Out Architecture with AWS Services - Use cases

This repository provides demo examples to implement a fan-out architecture for various use cases utilizing AWS services such as SNS (Simple Notification Service), SQS (Simple Queue Service), and Lambda. Each example includes a brief description of the implementation steps.

## Table of Contents

1. [Event-Driven Microservices](#event-driven-microservices)
2. [Real-Time Data Processing](#real-time-data-processing)
3. [Log Processing and Monitoring](#log-processing-and-monitoring)
4. [Workflow Orchestration](#workflow-orchestration)
5. [Data Replication and Synchronization](#data-replication-and-synchronization)
6. [Decoupled Systems for Scalability](#decoupled-systems-for-scalability)
7. [Data Aggregation](#data-aggregation)
8. [Notifications and Alerts](#notifications-and-alerts)
9. [Batch Processing](#batch-processing)
10. [Compliance and Auditing](#compliance-and-auditing)
11. [Conclusion](#conclusion)

## Event-Driven Microservices

### Example: User Registration Event

**Description:** Publish a user registration event to SNS, with multiple microservices subscribing to process the event.

**Implementation Steps:**
1. Create an SNS Topic: Create a topic named `UserRegistration`.
2. Create Microservices: Deploy two Lambda functions, `NotifyService` (for sending notifications) and `AnalyticsService` (for recording user data).
3. Subscribe Lambda Functions: Subscribe both Lambda functions to the `UserRegistration` topic.
4. Publish Event: When a user registers, publish a message to the `UserRegistration` topic containing user details.

---

## Real-Time Data Processing

### Example: Social Media Interactions

**Description:** Capture likes, shares, and comments in real-time.

**Implementation Steps:**
1. Create an SNS Topic: Create a topic named `UserInteractions`.
2. Create Lambda Functions: Deploy Lambda functions for processing likes, shares, and comments.
3. Subscribe Functions: Subscribe the Lambda functions to the `UserInteractions` topic.
4. Publish Event: Each interaction publishes a message to the `UserInteractions` topic.

---

## Log Processing and Monitoring

### Example: Application Log Management

**Description:** Publish application logs to an SNS topic for centralized processing.

**Implementation Steps:**
1. Create an SNS Topic: Create a topic named `ApplicationLogs`.
2. Create SQS Queues: Create two SQS queues, `MonitoringQueue` and `ArchivingQueue`.
3. Set Up Permissions: Add access policies to the SQS queues to allow SNS to send messages.
4. Subscribe Queues: Subscribe both queues to the `ApplicationLogs` topic.
5. Publish Logs: Send log messages to the `ApplicationLogs` topic.

---

## Workflow Orchestration

### Example: Order Processing Workflow

**Description:** Trigger multiple services based on a new order event.

**Implementation Steps:**
1. Create an SNS Topic: Create a topic named `NewOrder`.
2. Create Lambda Functions: Deploy functions for payment processing, inventory updates, and notification sending.
3. Subscribe Functions: Subscribe all functions to the `NewOrder` topic.
4. Publish Order Event: Publish a message with order details to the `NewOrder` topic when a new order is placed.

---

## Data Replication and Synchronization

### Example: Database Change Events

**Description:** Publish changes from a primary database to replicate data to secondary databases.

**Implementation Steps:**
1. Create an SNS Topic: Create a topic named `DatabaseChanges`.
2. Create Lambda Functions: Deploy functions for each secondary database that will handle changes.
3. Subscribe Functions: Subscribe each Lambda function to the `DatabaseChanges` topic.
4. Publish Change Events: Publish change events (insert/update/delete) to the `DatabaseChanges` topic.

---

## Decoupled Systems for Scalability

### Example: E-commerce Order System

**Description:** Process orders with multiple services independently.

**Implementation Steps:**
1. Create an SNS Topic: Create a topic named `OrderProcessing`.
2. Create Lambda Functions: Deploy functions for payment processing, shipping, and customer notifications.
3. Subscribe Functions: Subscribe all functions to the `OrderProcessing` topic.
4. Publish Order Event: Publish an order event to the `OrderProcessing` topic.

---

## Data Aggregation

### Example: Centralized Analytics System

**Description:** Aggregate data from various sources into a single analytics platform.

**Implementation Steps:**
1. Create an SNS Topic: Create a topic named `DataAggregation`.
2. Create Lambda Functions: Deploy functions that aggregate data from different sources.
3. Subscribe Functions: Subscribe the functions to the `DataAggregation` topic.
4. Publish Data Events: Publish data from various sources to the `DataAggregation` topic.

---

## Notifications and Alerts

### Example: System Alert Notifications

**Description:** Send alerts to multiple channels based on system events.

**Implementation Steps:**
1. Create an SNS Topic: Create a topic named `SystemAlerts`.
2. Subscribe Notification Channels: Subscribe Lambda functions or email/SMS endpoints to the `SystemAlerts` topic.
3. Publish Alert Events: Publish alerts to the `SystemAlerts` topic whenever a significant event occurs.

---

## Batch Processing

### Example: Batch Job Triggering

**Description:** Trigger batch jobs based on certain events.

**Implementation Steps:**
1. Create an SNS Topic: Create a topic named `BatchJobs`.
2. Create Lambda Functions: Deploy batch processing functions that handle the jobs.
3. Subscribe Functions: Subscribe the batch processing functions to the `BatchJobs` topic.
4. Publish Batch Events: Publish a message to trigger batch jobs to the `BatchJobs` topic.

---

## Compliance and Auditing

### Example: Audit Logging

**Description:** Ensure compliance by tracking important events.

**Implementation Steps:**
1. Create an SNS Topic: Create a topic named `AuditLogs`.
2. Create Lambda Functions: Deploy functions that log audit information.
3. Subscribe Functions: Subscribe the logging functions to the `AuditLogs` topic.
4. Publish Audit Events: Publish important events to the `AuditLogs` topic for tracking.

---

## Conclusion

These examples illustrate how to implement a fan-out architecture for various use cases using AWS services. Each use case highlights the flexibility and scalability that the fan-out pattern offers, enabling applications to respond to events in a decoupled manner.
