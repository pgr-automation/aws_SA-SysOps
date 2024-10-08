# Amazon SQS and SNS Integration

This guide explains how to create an Amazon SQS (Simple Queue Service) queue and subscribe it to an Amazon SNS (Simple Notification Service) topic.

## Step 1: Create an SNS Topic

1. Open the [Amazon SNS console](https://console.aws.amazon.com/sns).
2. In the navigation pane, choose **Topics**.
3. Click **Create topic**.
4. Choose the topic type (**Standard** or **FIFO**) and provide a **Name** for the topic.
5. Click **Create topic**.

## Step 2: Create an SQS Queue

1. Open the [Amazon SQS console](https://console.aws.amazon.com/sqs).
2. Click on **Create queue**.
3. Choose the queue type (**Standard** or **FIFO**) and provide a **Name** for your queue.
4. Configure the necessary settings, such as visibility timeout, message retention period, etc.
5. Click **Create queue**.

## Step 3: Subscribe the SQS Queue to the SNS Topic

1. Go back to the **Amazon SNS** console.
2. Click on the SNS topic you created.
3. In the **Subscriptions** tab, click on **Create subscription**.
4. For the **Protocol**, select **SQS**.
5. In the **Endpoint** field, paste the ARN of the SQS queue you created.
6. Click **Create subscription**.

## Step 4: Grant SNS Permission to Send Messages to SQS

To allow the SNS topic to send messages to the SQS queue, you need to add a policy to the SQS queue:

1. Open the **Amazon SQS** console and select your queue.
2. Click on the **Access policy** section in the **Permissions** tab.
3. Add the following policy to allow the SNS topic to send messages to the SQS queue:

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

   Replace `<region>`, `<account-id>`, `<queue-name>`, and `<topic-name>` with your specific values.

4. Save the policy.

## Conclusion

Your SQS queue is now subscribed to the SNS topic and will receive messages whenever a new message is published to the SNS topic.
