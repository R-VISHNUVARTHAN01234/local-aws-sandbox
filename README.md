# Local AWS Sandbox - Serverless Architecture with LocalStack

## Project Overview

This project simulates an AWS serverless architecture locally using Docker and LocalStack. It demonstrates how an Amazon S3 object upload event can automatically trigger an AWS Lambda function.

The project runs on my local machine without deploying these resources to a real AWS account.

## Architecture


User uploads a file
        |
        v
Amazon S3 (simulated)
        |
        | ObjectCreated event
        v
AWS Lambda (simulated)
        |
        | Processes event
        v
CloudWatch Logs (simulated)

## Technologies Used

- Docker: Runs the local cloud environment.
- LocalStack: Simulates AWS services locally.
- Amazon S3: Stores uploaded objects.
- AWS Lambda: Processes S3 upload events.
- CloudWatch Logs: Captures Lambda execution logs.
- AWS CLI and awslocal: Configure and test resources.
- Python: Implements the Lambda handler.
- PowerShell: Manages the workflow on Windows.

## Features

- Runs a local AWS-like environment using Docker.
- Creates and manages an S3 bucket.
- Uploads and downloads files from S3.
- Deploys a Python Lambda function.
- Configures automatic S3 object-created notifications.
- Processes upload events in the Lambda function.
- Verifies execution through logs.

## Project Structure

```text
local-aws-sandbox/
|-- docker-compose.yml
|-- lambda/
|   |-- app.py
|-- s3-notification.json
|-- sample.txt
|-- README.md
|-- .gitignore
```

## How It Works

1. Start the LocalStack container using Docker Compose.
2. Create an S3 bucket named `my-first-local-bucket`.
3. Deploy the Python Lambda function named `hello-local-lambda`.
4. Configure S3 to send object-created events to Lambda.
5. Upload a file to the bucket.
6. S3 automatically invokes the Lambda function.
7. Lambda reads the event and logs the bucket name and uploaded filename.

## Example Output

When a file is uploaded, the Lambda function logs a message similar to:

```text
New file uploaded: trigger-success-test.txt in bucket my-first-local-bucket
```

The function returns a response containing a success message and the number of event records processed.

## How to Test

After starting LocalStack and configuring the project resources, run these PowerShell commands to upload a test file:

```powershell
"Hello from my local cloud project!" | Set-Content .\test-upload.txt

docker cp .\test-upload.txt localstack-main:/tmp/test-upload.txt

docker exec localstack-main awslocal s3 cp /tmp/test-upload.txt s3://my-first-local-bucket/test-upload.txt
```

Check the Lambda log streams:

```powershell
docker exec localstack-main awslocal logs describe-log-streams --log-group-name /aws/lambda/hello-local-lambda --order-by LastEventTime --descending --max-items 3
```

Use a returned log stream name to inspect the execution logs.

## Learning Outcomes

- Understanding event-driven and serverless architecture.
- Learning how S3 events trigger Lambda functions.
- Working with Docker-based local cloud environments.
- Configuring service notifications and invocation permissions.
- Reading logs to troubleshoot and verify execution.
- Practising AWS CLI commands and Python event processing.

## Limitations

This project uses LocalStack to simulate AWS services locally. It is not a production deployment to real AWS infrastructure.

## Future Improvements

- Add automated tests for the Lambda handler.
- Add input validation and error handling.
- Store file metadata in DynamoDB.
- Explore Terraform for infrastructure as code.
- Add a GitHub Actions workflow for automated checks.

## Author

Cloud and DevOps learning project built as part of my hands-on cloud engineering practice.
