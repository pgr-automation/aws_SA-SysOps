# Integrate Amazon SQS with API Gateway

This guide explains how to integrate Amazon SQS with API Gateway directly, allowing you to create an API that sends messages to an SQS queue without requiring an intermediary service like AWS Lambda.

## Prerequisites
- An AWS account with appropriate permissions to manage SQS, IAM, and API Gateway.
- Familiarity with AWS Management Console.

## Steps to Integrate SQS with API Gateway

### 1. Create an SQS Queue
1. Open the **AWS Management Console**.
2. Navigate to the **Amazon SQS** service.
3. Create a new **Standard Queue**.
4. Note down the **Queue URL** for later use.

### 2. Create an IAM Role for API Gateway
1. Open the **IAM** console.
2. Create a new **Role** with the service type set to **API Gateway**.
3. Attach the **AmazonSQSFullAccess** policy or a custom policy that includes permissions to send messages to your specific SQS queue.

### 3. Create a REST API in API Gateway
1. Open the **API Gateway** console.
2. Create a new **REST API**.
3. Add a new **Resource Path** (e.g., `/send-message`).
4. Under the `/send-message` resource, create a new **POST** method.

### 4. Configure the Integration Request
1. Set the **Integration Type** to **AWS Service**.
2. Configure the following settings:
   - **AWS Region**: The region where your SQS queue is located.
   - **AWS Service**: `SQS`.
   - **HTTP Method**: `POST`.
   - **Action**: `SendMessage`.
   - **Execution Role**: Enter the ARN of the IAM role created for API Gateway.

## Additional Configuration (Optional)
- You can add request validation and mapping templates to ensure the correct format of the data sent to the SQS queue.
- Configure CORS settings for the API if you plan to access it from a web application.

## Testing the API
- Once configured, test the API using the **API Gateway console** or tools like **Postman** to ensure that messages are being successfully sent to the SQS queue.

## Cleanup
- Delete the resources created (API Gateway, SQS queue, IAM role) if they are no longer needed to avoid incurring charges.

## References
- [Amazon SQS Documentation](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)
- [API Gateway Documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)
- [IAM Roles for API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-setup-iam.html)


```
SQS apigateway 
1 . Create sqs 
2. create apigateway
3 . create IAM role with 
	1. cloudwatch logs
	2. sqs full permission
4. create routes for put and get methods
5 . Integrate methods with sqs



$request.body.MessageBody


      
      
Post msg: 

curl -X POST   https://kw1elhyfjc.execute-api.us-east-1.amazonaws.com/send-message   -H "Content-Type: application/json"   -d '{
        "Action": "SendMessage",
        "QueueUrl": "https://sqs.us-east-1.amazonaws.com/851725172472/apigatewayqueue",
        "MessageBody": "This is a test message one."
      }'
      
Receive msg
aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/851725172472/apigatewayqueue --region=us-east-1
```