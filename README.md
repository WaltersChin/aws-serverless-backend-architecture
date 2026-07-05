# aws-serverless-backend-architecture
Serverless Backend Architecture on AWS
This project demonstrates the design and implementation of a serverless backend architecture using Amazon API Gateway, AWS Lambda and Amazon DynamoDB. The solution exposes a REST API capable of performing CRUD operations while leveraging fully managed AWS services to reduce operational overhead, support automatic scaling and enable pay-per-use pricing. The architecture follows core cloud design principles including serverless computing, event-driven processing, least-privilege security and operational scalability

## Architecture Diagram

images/architecture-diagram.png
``
## Architecture Overview:
The solution consists of the following AWS services:
•	Amazon API Gateway serves as the public HTTPS endpoint.
•	AWS Lambda contains the application logic and processes CRUD requests.
•	Amazon DynamoDB stores application data using a serverless NoSQL database.
•	AWS IAM provides secure, least-privilege access between services.
•	Amazon CloudWatch captures logs and operational metrics for monitoring and troubleshooting.
