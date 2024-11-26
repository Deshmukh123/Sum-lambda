# Sum-lambda


1. Input Struct: This struct (`Input`) is used to capture the incoming JSON body. It expects two fields: `num1` and `num2`, which are integers.
   
2. Response Struct: The `Response` struct is used to format the output. This will be returned in the JSON response from the Lambda function.

3. Handler Function: The `handler` function is the core of the Lambda function. It:
   - Takes an `Input` struct as the argument, which is the decoded JSON payload.
   - Adds `num1` and `num2` together.
   - Returns a `Response` struct containing the result of the addition.

4. Lambda Start: The `lambda.Start(handler)` function is a part of the AWS Lambda Go SDK and will invoke the `handler` function when the Lambda is triggered by an event (e.g., an API Gateway request).

---

### Steps to Build, Package, and Deploy:

1. Install AWS Lambda Go SDK:
   First, make sure the AWS Lambda Go SDK is installed:

   ```bash
   go get github.com/aws/aws-lambda-go/lambda
   ```

2. Build the Go Code for Lambda:
   You need to build the Go binary for Linux because AWS Lambda runs on Linux environments.

   Run this command in your project directory:

   ```bash
   GOOS=linux GOARCH=amd64 go build -o main main.go
   ```

3. Create a ZIP File:
   After building the Go binary, you need to create a ZIP file that contains the `main` binary. You can use PowerShell’s `Compress-Archive` to create the ZIP file:

   ```powershell
   Compress-Archive -Path .\main -DestinationPath .\main.zip
   ```

4. Upload the Lambda Function:
   Use the AWS CLI to upload the function to AWS Lambda. Make sure you have an appropriate IAM role for the Lambda function (the role should have the `AWSLambdaBasicExecutionRole` policy):

   ```bash
   aws lambda create-function --function-name MyGoFunction \
     --zip-file fileb://main.zip \
     --handler main \
     --runtime go1.x \
     --role arn:aws:iam::<YOUR_ACCOUNT_ID>:role/lambda-role
   ```

   - Replace `<YOUR_ACCOUNT_ID>` with your AWS account ID.
   - Replace `lambda-role` with the IAM role ARN that grants permission to execute the Lambda function.

---

### Testing the Lambda Function:

1. Create an API Gateway:
   Set up an API Gateway to trigger your Lambda function. This will expose your Lambda function as a REST API that can be invoked via HTTP requests.

2. Invoke the Lambda Function:
   You can test the function using Postman or curl. Here's an example `curl` request:

   ```bash
   curl -X POST https://your-api-id.execute-api.us-east-1.amazonaws.com/prod/add \
     -H "Content-Type: application/json" \
     -d '{"num1": 5, "num2": 7}'
   ```

   You should receive a JSON response like:

   ```json
   {
     "result": 12
   }
   ```

---

### Summary:

1. Code Fix: This is a fully executable and Lambda-compatible Go function.
2. Deployment Steps: You need to build the Go binary for Linux, create a ZIP file, and deploy it using the AWS CLI.
3. Test: Use API Gateway to test the Lambda function with Postman or curl.

Let me know if you encounter any issues during the deployment or testing process!
