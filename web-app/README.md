## Building an End-to-End AWS Web Application from Scratch
#### The purpose of this project is to showcase my abilities of choosing the most suitable technologies and employing various cloud architecture concepts and best practices to build scalable and secure applications in a production environment. By using my passion and creativity in this project, it'll prepare my understanding and my skillset for real-world challenges in this rapidly evolving industry.

In this project, I will be architecting and building a fully functioning End-to-End AWS Web Application that follows the following steps:

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/web-app/Screenshots/web%20architecture.PNG)

1. **AWS Amplify** hosts the website and communicates to the client user
   
2. **API Gateway** will send information from Amplify to AWS Lambda
   
3. **AWS Lambda** to process the user-based information
   
4. AWS Lambda will send the processed data to a NoSQL database (DynamoDB)
 
5. **DynamoDB** receives the data and stores the data for the client user.

AWS Lambda will be configured with the apprioriate IAM role to be able to access tbe DynamoDB table for security purposes while using least privilege policies.

The joy of this kind of project is that this web application could host anything and the possibilites are endless. 
In this specific case, what's more important to me is showcasing the fludity of connections between various AWS products and this specific web application will be a calculator that stores 
the values of user-inputted numbers powered to a chosen exponent.

-----
To start, I prepared an index.hmtl file to display the contents of my calculator which can be found in the repository. I need a way to host this website as well as allow for communication 
between the user and any specific target. This can be easily done using AWS Amplify to host and deploy the website.

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/web-app/Screenshots/Amplify-Deployment.png)

Once the deployment is complete, I can use the domain url to go to the website anytime. But now it needs to be able to calculate the values that the user inputs.

The easiest and serverless solution to compute values is by using AWS Lambda. Once triggered, the lambda function I created will take a base and exponent value from the user and calculate it. 
A test event with a base of 2 and an exponent of 3 was ran to confirm completion and this is the result:

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/web-app/Screenshots/TestingLambdaResults.png)

To invoke the Lambda function, it need to be connected to our web application from AWS Amplify as the trigger. This was done by using AWS API Gateway and creating a REST API. 
The REST API will wait for a request from Amplify to take user-inputted parameters and once the request is received, it will invoke a response from Lambda to take the data and process it.

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/web-app/Screenshots/deploying-API.png)

AWS Amplify also needs to connect to AWS API Gateway. This connection is done by directly invoking the REST API in my index.html.

With this complete and the lambda function as the target, I created another test event through AWS Amplify and Lambda responded with the correctedly calculated values, indicating the successful connection between Amplify and Lambda.

From here, in order for AWS Lambda to commit the process data to a DynamoDB table, it needs to be set with a role that has read/write priliveges. 
By initializing using AWS IAM, I configured a role for the Lambda Function that has incorporates least priliveges policy for security. 

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/web-app/Screenshots/IAMPolicyCreation.png)

With the permissions set for the execution role for Lambda,  I created a DynamoDB table that will hold the processed values and the LambdaFunction is able to read/write to it.

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/web-app/Screenshots/web-app-launched.png)

The results of 2^8 shown here: 

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/web-app/Screenshots/value-saved-dynamo.png)

With that set up, the End-to-End web application is officially built, and completely connected, incorporating over five individual AWS products. The web application is able to take user input, invoke a Lambda function to calculate the data, and store the value in a DynamoDB table! 

-----

What I loved most about this project is that it allow for an immense amount of freedom to be able to wield these services and produce infinite possible solutions. It allowed me to implement crucial elements to other real-world scenarios such as:

1. Implementing continuous CI/CD and Devops by setting up automated deployment pipelines, version control, and monitoring solutions.
2. Allowed for scalability and performance testing and understanding how the application performs under different loads and for implementing optimizations to ensure optimal user experience.
3. Configuring security implementations at various levels, including network security, data encryption, access controls, and compliance with best security practices.
4. Integrating AWS Services, allowing for a cohesive and scalable architecture

-----
**References**

https://www.youtube.com/watch?v=7m_q1ldzw0U&t=67s
