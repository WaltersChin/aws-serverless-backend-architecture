# aws-serverless-backend-architecture
Serverless Backend Architecture on AWS
This project demonstrates the design and implementation of a serverless backend architecture using Amazon API Gateway, AWS Lambda and Amazon DynamoDB. The solution exposes a REST API capable of performing CRUD operations while leveraging fully managed AWS services to reduce operational overhead, support automatic scaling and enable pay-per-use pricing. The architecture follows core cloud design principles including serverless computing, event-driven processing, least-privilege security and operational scalability

## Architecture Diagram

images/architecture-diagram.png
``
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
##Create Custom Policy
We need to create a custom policy for least privilege
1)To enforce least privilege access, create a custom IAM policy by navigating to the Policies page in the IAM console. Select Create policy, switch to the JSON editor and paste the required policy definition to precisely control permissions. 

