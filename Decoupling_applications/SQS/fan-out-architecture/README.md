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