# aws-serverless-backend-architecture
## Serverless Backend Architecture on AWS

This project demonstrates the design and implementation of a serverless backend architecture using Amazon API Gateway, AWS Lambda and Amazon DynamoDB. The solution exposes a REST API capable of performing CRUD operations while leveraging fully managed AWS services to reduce operational overhead, support automatic scaling and enable pay-per-use pricing. The architecture follows core cloud design principles including serverless computing, event-driven processing, least-privilege security and operational scalability.
## Lab Overview and High-Level Design
<img width="1204" height="624" alt="high-level-design jpg" src="https://github.com/user-attachments/assets/be592f54-b3bc-40ec-9536-f46e998ca640" />


## Architecture Overview
The solution consists of the following AWS services:
•	Amazon API Gateway serves as the public HTTPS endpoint.
•	AWS Lambda contains the application logic and processes CRUD requests.
•	Amazon DynamoDB stores application data using a serverless NoSQL database.
•	AWS IAM provides secure, least-privilege access between services.
•	Amazon CloudWatch captures logs and operational metrics for monitoring and troubleshooting.
## Request Flow
1.	Client sends an HTTPS POST request to Amazon API Gateway.
2.	Amazon API Gateway routes the request to AWS Lambda.
3.	AWS Lambda processes the requested CRUD operation.
4.	AWS Lambda reads from or writes to Amazon DynamoDB.
5.	The response is returned to the client through Amazon API Gateway.
6.	Amazon CloudWatch captures logs and metrics for monitoring and troubleshooting.
## AWS Component setup
Create Custom Policy
We need to create a custom policy for least privilege
1)To enforce least privilege access, create a custom IAM policy by navigating to the Policies page in the IAM console. Select Create policy, switch to the JSON editor and paste the required policy definition to precisely control permissions. 
<img width="891" height="447" alt="create-policy" src="https://github.com/user-attachments/assets/df25e591-f59d-45e2-8e1b-e4241105d9d9" />



## 2 Create Lambda Execution Role (Least Privilege Access)
To provision the Lambda function with only the permissions it requires, create a least privilege IAM execution role. From the IAM console, open the Roles section, choose Create role, select AWS Service → Lambda as the trusted entity, and attach the previously created lambda-custom-policy. This ensures the function can access only the AWS resources explicitly defined in the policy.
<img width="970" height="378" alt="create-lambda-role" src="https://github.com/user-attachments/assets/b3c1ebb4-61fa-42ed-854d-8e5f7e2870a6" />

o	Role name – lambda-apigateway-role.
o	Click "Create role"
## Create Lambda Function
## To create the function
1.	Click "Create function" in AWS Lambda Console
<img width="944" height="184" alt="create-lambda" src="https://github.com/user-attachments/assets/ea9bf933-dc03-429c-abda-f2c10ff065bf" />


2.	Select "Author from scratch". For example Use name LambdaFunctionOverHttps, select Python 3.13 or the version that works for you as Runtime. Under Permissions, click the arrow beside "Change default execution role", then "use an existing role" and select lambda-apigateway-role, from the drop down.
3.	## Create Function.
   <img width="978" height="631" alt="lambda-basic-info" src="https://github.com/user-attachments/assets/5426badd-a49a-431a-84c9-33bae80a8b42" />


4 . Replace the boilerplate coding with the following code snippet and click "Deploy"
<img width="975" height="501" alt="lambda-code-paste" src="https://github.com/user-attachments/assets/53fac600-5997-4bf9-bdc2-a111606801ce" />

## Test the Lambda Function (Initial Echo Validation)
Before integrating DynamoDB or API Gateway, validate the Lambda function using a simple echotest. Open the Test tab in the Lambda console, create a new event (for example echotest), and provide a JSON payload. The operation field determines the action, and in this phase the function should simply return the input payload, confirming that the handler executes correctly.
{
    "operation": "echo",
    "payload": {
        "somekey1": "somevalue1",
        "somekey2": "somevalue2"
    }
}
4.	After running the test event successfully, it will produce the output below
<img width="476" height="230" alt="execute-test jpg" src="https://github.com/user-attachments/assets/9dc89b65-6907-4a29-8c21-0d4264f6be0f" />


 ## Create DynamoDB Table (Backend Data Store)
With the Lambda function validated, the next step is provisioning the DynamoDB table it will use. Open the DynamoDB console, navigate to Tables, and select Create table. Create a table named lambda-apigateway with a partition key of id (String), then finalize by choosing Create table.

<img width="1013" height="396" alt="create-dynamo-table" src="https://github.com/user-attachments/assets/8d64c50a-a498-45a9-9bb8-c1b593bca336" />

## 5.Create the REST API (Lambda Backed)
Provision the API by opening the API Gateway console, selecting Create API, and choosing Build under REST API. Name the API DynamoDBOperations, retain the default settings, and finalize by selecting Create API.
<img width="1032" height="457" alt="create-new-api" src="https://github.com/user-attachments/assets/9a5eaa78-136a-44dc-afa2-065dc2901b0b" />

 
 
## 6.Add API Resource (Logical Routing Structure)
An API consists of resources and methods organized into a hierarchical structure that reflects application logic. To begin building this structure, add a new resource under the root by selecting Create Resource and naming it DynamoDBManager. This establishes the logical entry point for DynamoDB related operations within the API.

<img width="1009" height="243" alt="create-resource-name" src="https://github.com/user-attachments/assets/bc56146e-3a6a-4a14-bceb-94aaf8353684" />


## 7.Let's create a POST Method for our API. With the "/dynamodbmanager" resource selected, click "Create Method".
<img width="439" height="102" alt="create-method-1 jpg" src="https://github.com/user-attachments/assets/ec245ca0-b330-44c0-b7ae-9ba4fb6b670c" />
 
## 8.Configure POST Method (Lambda Integration)
Add a POST method to the resource and integrate it with your Lambda backend. Select POST from the method dropdown, keep Lambda Function as the integration type, and choose the previously created LambdaFunctionOverHttps. Once selected, confirm the configuration by choosing Create method.
<img width="1041" height="463" alt="create-lambda-integration" src="https://github.com/user-attachments/assets/a9e7139d-2129-4fcd-8984-6ccb7bb5befe" />
## 9.Deploy the API (Production Stage)
With the Lambda integration complete, deploy the API to a production stage. Choose Deploy API in the API Gateway console, select New Stage, name it Prod, and finalize the deployment. This publishes the API to a stable, versioned endpoint ready for external consumption.

<img width="336" height="370" alt="deploy-api-2" src="https://github.com/user-attachments/assets/c5771146-8208-4198-98a2-eb9f32109eaa" />

To invoke the API endpoint, we need the endpoint url. In the "Stages" screen, expand the stage "Prod", keep expanding till you see "POST", select "POST" method and copy the "Invoke URL" from screen
<img width="384" height="93" alt="copy-invoke-url jpg" src="https://github.com/user-attachments/assets/aa386e81-2613-434e-bdcd-a071904105f8" />

## 10.Run the Solution (Create Operation Request)
To execute the solution, the Lambda function supports a create operation that writes an item to DynamoDB. Trigger this behavior by sending a JSON payload that specifies the desired operation, allowing Lambda to process the request and store the data accordingly.

<img width="406" height="169" alt="Create-Lambda-gateway jpg" src="https://github.com/user-attachments/assets/61a5d9b9-edfc-4411-9ad2-123f8e9e1996" />

API can be executed locally using either Postman or Curl. In Postman, choose POST, paste the API URL, switch the Body to raw, insert the JSON payload, and hit Send. A successful call returns HTTPStatusCode 200.

<img width="540" height="429" alt="create-from-postman jpg" src="https://github.com/user-attachments/assets/629b7562-d341-4eae-a3c7-84b3f33916d6" />

 
## To run this from a terminal using Curl, run the below code
$ curl -X POST -d "{\"operation\":\"create\",\"tableName\":\"lambda-apigateway\",\"payload\":{\"Item\":{\"id\":\"1\",\"name\":\"Bob\"}}}" https://$API.execute-api.$REGION.amazonaws.com/prod/DynamoDBManager
11.To can confirm that the item was added to DynamoDB by opening the DynamoDB console, selecting the lambda apigateway table, choosing Explore table items, and checking that the new entry appears.
<img width="968" height="391" alt="dynamo-item jpg" src="https://github.com/user-attachments/assets/9838167b-e6d1-467b-8a3c-9650b23aa120" />

12.To retrieve all items in the DynamoDB table by using the Lambda API’s list operation. Send the below provided JSON payload to the same API, and it will return every stored item.

<img width="537" height="427" alt="dynamo-item-list jpg" src="https://github.com/user-attachments/assets/d3609f3a-bc54-4ff6-856b-a31fd68acd4b" />
## Conclusion
This project demonstrates how AWS managed services can be combined to build a scalable, secure, and cost-effective serverless application without managing any underlying infrastructure.
By integrating Amazon API Gateway, AWS Lambda, and Amazon DynamoDB, I designed and implemented a RESTful API capable of performing CRUD operations while applying core cloud architecture principles such as least-privilege access, event-driven processing, and serverless scalability.
This solution highlights the benefits of serverless architecture, including reduced operational overhead, automatic scaling and pay-per-use pricing. The architecture can be further enhanced with authentication, Infrastructure as Code (Terraform/CloudFormation), CI/CD automation and advanced monitoring to support enterprise-grade production workloads.

## Key Learnings
•	Designed and deployed a serverless application using Amazon API Gateway, AWS Lambda, and Amazon DynamoDB.
•	Applied IAM security best practices through a custom least-privilege execution role and policy.
•	Implemented CRUD operations through a single Lambda function integrated with API Gateway.
•	Gained hands-on experience with API integration patterns and event-driven architectures.
•	Utilized Amazon CloudWatch for monitoring, logging, and troubleshooting application behavior.
•	Strengthened understanding of secure, scalable, and highly available cloud-native application design.
## Future Enhancements
•	Implement Amazon Cognito authentication and authorization.
•	Deploy infrastructure using Terraform or AWS CloudFormation.
•	Implement CI/CD pipelines using GitHub Actions.
•	Add API request validation and error handling.
•	Enhance monitoring and alerting with CloudWatch dashboards and alarms.
•	Perform performance and load testing using Postman and other testing tools.
•	Optimize Lambda performance and cost using AWS Lambda Power Tuning.
•	Introduce API versioning and throttling policies for production readiness.
## Author
This project showcases the design and implementation of a serverless AWS solution using Amazon API Gateway, AWS Lambda and Amazon DynamoDB. The solution emphasizes cloud architecture best practices, including least-privilege security, serverless scalability, event-driven processing, and operational simplicity.

## Technologies Used:
•	Amazon API Gateway
•	AWS Lambda
•	Amazon DynamoDB
•	AWS IAM
•	Amazon CloudWatch
•	Python
