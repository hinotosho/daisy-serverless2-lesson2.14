# daisy-serverless2-lesson2.14
CE9 Lesson 2.14 Homework 

1. Does SNS Guarantee Exactly-Once Delivery to Subscribers?
   
No, Amazon SNS (Simple Notification Service) does not guarantee exactly-once delivery. Instead, it provides at-least-once delivery for most transport protocols, meaning a message may be delivered more than once to subscribers2.

For Amazon SQS and AWS Lambda subscribers, SNS ensures at-least-once delivery.

For HTTP/S endpoints, SNS retries failed deliveries but does not guarantee exactly-once delivery.

For FIFO SNS topics, ordering is preserved, but duplicate messages may still occur.

Since SNS follows a publish-subscribe model, it prioritizes high availability and durability over strict deduplication.

2. What is the Purpose of the Dead-Letter Queue (DLQ)?
   
A Dead-Letter Queue (DLQ) is a special queue that stores messages that could not be processed successfully.

Why is a DLQ Important?
Prevents message loss by capturing failed messages.

Helps debug issues by storing problematic messages for analysis.

Avoids queue overflow, ensuring failed messages don’t block normal processing.

How Does a DLQ Work?
If a message fails to be processed after multiple retries, it is moved to the DLQ.

Developers can inspect the DLQ to identify errors and manually process or delete messages.

DLQs are available for Amazon SQS, SNS, and EventBridge, making them useful for handling failures in event-driven architectures.

3. How Would You Enable a Notification to Your Email When Messages Are Added to the DLQ?
   
To receive email notifications when messages are added to a DLQ, follow these steps6:

Step 1: Create an Amazon SNS Topic
Go to AWS Console → SNS → Create Topic.

Choose Standard Topic and name it (e.g., DLQ-Alerts).

Click Create Topic.

Step 2: Subscribe Your Email to the SNS Topic
In the SNS topic, click Create Subscription.

Choose Protocol: Email.

Enter your email address and click Create Subscription.

Check your inbox and confirm the subscription.

Step 3: Set Up an Amazon CloudWatch Alarm for DLQ

Go to AWS Console → CloudWatch → Alarms.

Click Create Alarm.

Select SQS Metrics → ApproximateNumberOfMessagesDelayed (for DLQ).

Set a threshold (e.g., greater than 0 messages).

Choose SNS Topic (DLQ-Alerts) as the notification target.

Click Create Alarm.

Now, whenever a message is added to the DLQ, CloudWatch will trigger an SNS notification, sending an email alert.
