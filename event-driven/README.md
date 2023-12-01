![image](https://github.com/Seanhui2000/AWS-Projects/assets/127706776/947e7d6e-06da-472b-913f-0f1b064edcee)## Building an event driven architecture in AWS

In this project, I will be designing and building an event driven architecture in AWS that waits for a function to be invoked and once fired, 
Lambda destinations will be asynchronously configured to send an event, or in this case execution record, that contains details about the request and response. 
This architecture will be utilizing AWS EventBridge, Step Functions, SNS, and AWS Lambda.

------
T
This will be the event driven architecture design for this project provided from the workshop reference.
![alt text]()
To start, I need to set up an event bus that will take a notification event, in this case an "order", and write it to CloudWatch to log all events that occured. This was done by using AWS EventBridge to create an event bus, set rules to configure the source, and set the target to a CloudWatch group log. Once the setup is complete, I could test it with a test event in EventBridge using the following example JSON. 

```JSON
{
   "category": "lab-supplies",
   "value": 415,
   "location": "eu-west"
}
```
![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/event-driven/Screenshots/eventbus%20creation.png)

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/event-driven/Screenshots/test-event-logs.png)

As you can see, an order notification was successfully delivered so the event bus is working perfectly. 

Now with this Order event bus, I implemented another EventBridge rule to named EUOrdersRule to process only orders from locations in EU using an AWS Step Functions called OrdersProcessing. This step function will be important for connecting the Lambda function later on. Once this was implemented, I tested it with another test event with an EU location and it pushed the notification out successfully.

```JSON
{ "category": "office-supplies", "value": 300, "location": "eu-west" }
```

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/event-driven/Screenshots/test-event.png)

With this configured Order event bus and the AWS Step Functions "OrderProcessing", I can connect them all with a Lambda function to send an execution record to another event bus. The execution record will contain details about the request and response in JSON format including version, timestamp, request context, request payload, response context, and response payload.

To start, I set up another event bus called Inventory that will be the destination of the execution record. I also need to set up the Lambda function called InventoryFunction in AWS Lambda that will be invoked upon receiving a notification. In AWS Lambda, by using Lambda destinations, I set the InventoryFunction's destination to be the event bus Inventory in AWS EventBridge.

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/event-driven/Screenshots/Connected-Lambda-to-EventBridge.png)

Once the destination was set, I specified a rule in the Orders event bus that handles the orders processed by the Step Function configured previously and the target will be the InventoryFunction in Lambda.

The final event driven architecture will look like this:

1. The message containing an EU location triggers the rule on the Orders event bus to route the message to the OrderProcessing StepFunctions workflow.

2. The OrderProcessing StepFunctions workflow processes the order and publishes an event to the Orders event bus.

3. The OrderProcessingRule on the Orders event bus routes the event to the Inventory function.

4. On successful execution, the InventoryFunction function sends an event to the Inventory event bus through the On success Lambda destination.

5. The InventoryDevRule rule on the Inventory event bus logs the event to the /aws/events/inventory CloudWatch Logs log group.

To test it, I simply set an event test again in EventBridge using this JSON script:

```JSON
{
  "category": "office-supplies",
  "value": 1200,
  "location": "eu-west",
  "OrderDetails": "completed"      
}

```

The event test will successfuly be published and the log group will read our execution record as expected!

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/event-driven/Screenshots/Success-logs.png)

The following is all the created event busses and rules to create this architecture:

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/event-driven/Screenshots/event-busses.png)

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/event-driven/Screenshots/RUles.png)

-----

By building an event driven architecture in Lambda, I was able understand in depth about multiple services' functionalities such as utilizing Lambda destinations to automatically send events to a specific destination and how a serverless event bus service in AWS can facilitate communication between different services by allowing events to be routed to different targets. I also learned a crucial element of best policy cloud architecture which in this case was the benefits of decoupling components in a serverless architecture, promoting scalability, flexibility, and easier maintenance.

-----
**References**

https://catalog.us-east-1.prod.workshops.aws/workshops/63320e83-6abc-493d-83d8-f822584fb3cb/en-US/lambda/destinations
