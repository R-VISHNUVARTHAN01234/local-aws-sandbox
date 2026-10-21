# Local AWS Sandbox - Serverless Architecture with LocalStack



## Project Overview



This project simulates an AWS serverless architecture locally using Docker and LocalStack. It demonstrates how an Amazon S3 object-upload event can automatically trigger an AWS Lambda function.



The project runs on my local machine without deploying these resources to a real AWS account.



## Architecture



```text

User uploads a file

&#x20;       |

&#x20;       v

&#x20; Amazon S3 (simulated)

&#x20;       |

&#x20;       | ObjectCreated event

&#x20;       v

&#x20;AWS Lambda (simulated)

&#x20;       |

&#x20;       | Processes event

&#x20;       v

&#x20;CloudWatch Logs (simulated)

```



## Technologies Used



\- Docker ÃƒÂ¢Ã¢â€šÂ¬Ã¢â‚¬Â runs the local cloud environment.

\- LocalStack ÃƒÂ¢Ã¢â€šÂ¬Ã¢â‚¬Â simulates AWS services locally.

\- Amazon S3 ÃƒÂ¢Ã¢â€šÂ¬Ã¢â‚¬Â stores uploaded objects.

\- AWS Lambda ÃƒÂ¢Ã¢â€šÂ¬Ã¢â‚¬Â processes S3 upload events.

\- CloudWatch Logs ÃƒÂ¢Ã¢â€šÂ¬Ã¢â‚¬Â captures Lambda execution logs.

\- AWS CLI / awslocal ÃƒÂ¢Ã¢â€šÂ¬Ã¢â‚¬Â creates, configures and tests resources.

\- Python ÃƒÂ¢Ã¢â€šÂ¬Ã¢â‚¬Â implements the Lambda handler.

\- PowerShell ÃƒÂ¢Ã¢â€šÂ¬Ã¢â‚¬Â manages the development workflow on Windows.



## Features



\- Runs a local AWS-like environment using Docker.

\- Creates and manages an S3 bucket.

\- Uploads and downloads files from S3.

\- Deploys a Python Lambda function.

\- Configures automatic S3 object-created notifications.

\- Processes upload events in the Lambda function.

\- Verifies execution through logs.



## Project Structure



```text

local-aws-sandbox/

ÃƒÂ¢Ã¢â‚¬ÂÃ…â€œÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ docker-compose.yml

ÃƒÂ¢Ã¢â‚¬ÂÃ…â€œÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ lambda/

ÃƒÂ¢Ã¢â‚¬ÂÃ¢â‚¬Å¡   ÃƒÂ¢Ã¢â‚¬ÂÃ¢â‚¬ÂÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ app.py

ÃƒÂ¢Ã¢â‚¬ÂÃ…â€œÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ s3-notification.json

ÃƒÂ¢Ã¢â‚¬ÂÃ…â€œÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ sample.txt

ÃƒÂ¢Ã¢â‚¬ÂÃ…â€œÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ README.md

ÃƒÂ¢Ã¢â‚¬ÂÃ¢â‚¬ÂÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ÃƒÂ¢Ã¢â‚¬ÂÃ¢â€šÂ¬ .gitignore

```



## How It Works



1\. Start the LocalStack container using Docker Compose.

2\. Create an S3 bucket named `my-first-local-bucket`.

3\. Deploy the Python Lambda function named `hello-local-lambda`.

4\. Configure S3 to send object-created events to Lambda.

5\. Upload a file to the bucket.

6\. S3 automatically invokes the Lambda function.

7\. The Lambda function reads the event, identifies the bucket and uploaded filename, and writes the result to its logs.



## Example Output



When a file is uploaded, the Lambda function logs a message similar to:



```text

New file uploaded: trigger-success-test.txt in bucket my-first-local-bucket

```



The function returns a response containing a success message and the number of event records processed.



## How to Test



After starting LocalStack and configuring the project resources, upload a test file:



```powershell

"Hello from my local cloud project!" | Set-Content .\\test-upload.txt



docker cp .\\test-upload.txt localstack-main:/tmp/test-upload.txt



docker exec localstack-main awslocal s3 cp /tmp/test-upload.txt s3://my-first-local-bucket/test-upload.txt

```



Check the Lambda log group:



```powershell

docker exec localstack-main awslocal logs describe-log-streams --log-group-name /aws/lambda/hello-local-lambda --order-by LastEventTime --descending --max-items 3

```



Then retrieve the newest log stream's events using the stream name returned by the command.



## Learning Outcomes



\- Understanding event-driven and serverless architecture.

\- Learning how S3 events trigger Lambda functions.

\- Working with Docker-based local cloud environments.

\- Configuring service notifications and invocation permissions.

\- Reading logs to troubleshoot and verify execution.

\- Practising AWS CLI

