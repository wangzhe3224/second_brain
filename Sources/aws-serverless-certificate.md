---
title: AWS Serverless Certificate
date: 2023-01-30
tags: 
    - aws
    - serverless
---

> An architect is designing a real-time chat module for a new online game platform and wants to use the WebSocket API capabilities within Amazon API Gateway. An important requirement for this chat system is the ability to track the current number of active users on the platform.

Which of the options below could help implement this functionality?

- Use the $default route to trigger a Lambda function that increments an active user count in DynamoDB.
- Use the predefined $active route when a connection is activated to trigger a Lambda function that updates an active users counter stored in a DynamoDB table.
- Use the `active-connections` command on the @connections API to periodically retrieve the total number of active connections and store this as a count in a DynamoDB table.
- [x] Use the `$connect` and `$disconnect` predefined routes to trigger a Lambda Function which increments and decrements an active user counter in DynamoDB respectively.

> As a Solutions Architect for an e-commerce company, you are asked to improve the overall performance of an application that is behind Amazon API Gateway. You also need to protect the backend system and the application from traffic spikes.

What is the optimal solution that you would recommend?

- Enable API Gateway to automatically scale and set throttling limits
- Enable caching and WAF (Web Application Firewall)
- [x] Enable caching and set throttling limits.
- Enable caching and scale API gateway services.

> A large company is planning to adopt serverless. To begin, they want to migrate a maintenance script written in Python that is run nightly.

Which of the following services could they use as part of the migration? (Select TWO)

- Amazon 
- [x] EventBridge
- Amazon SNS
- AWS Batch
- [x] AWS Lambda
- Amazon Simple Queue Service

> As a Solution Architect working for an online gaming company, you have been informed the player feedback system is slow to respond during peak periods. The feedback is reviewed weekly by the game developers. The current system submits the feedback to an Amazon API Gateway which invokes a Lambda function to first validate and then store the review in a Amazon DynamoDB table. This table has been configured with provisioned capacity.

Change Amazon DynamoDB from provisioned capacity to on-demand allowing the table to scale up to handle any increases in demand.

> How do you mitigate the AWS Lambda cold starts?

Prevent AWS Lambda cold starts with Lambda provisioned concurrency.

> You are a tech lead for a team responsible for launching a new location services API for a contact management system, the location data is stored in a relational database running on Amazon RDS.
> Your security team requires that credentials used to connect to databases must be encrypted at rest and rotated automatically every 90 days. All the activities associated with secrets rotation must be logged.

Which AWS service would allow you to implement this requirement with the least amount of overhead?

AWS Config Rule secretsmanager-secret-periodic-rotation

> You are a cloud architect working on modernizing an application for a customer. You decide to use managed services from AWS as a part of the application design.

How would you explain managed services from AWS to your customer?

A service managed by AWS.

> You are an application developer that is tasked to extend an existing backend application with a serverless API behind an Amazon API Gateway. Frontend clients will interact with the API of the existing application and the new serverless API via REST calls. Requests to both APIs are authenticated using a token in the HTTP header verified by the existing IAM solution.

How can you use the existing auth mechanism to secure access to the API Gateway with the least amount of work?

Create an Amazon Cognito Identity Pool and add your custom solutions as a federated identity provider. Use the native integration of Cognito with API Gateway for authentication

> 