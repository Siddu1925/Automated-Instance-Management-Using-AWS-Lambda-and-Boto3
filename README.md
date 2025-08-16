# Automated-Instance-Management-Using-AWS-Lambda-and-Boto3

Objective : Automates the Start and Stop instances states based on the individual tags assigned using AWS Lambda function and Boto3.

Steps:
1.EC2 Instance setup
  Create two EC2 instances.
Tag one with Action=Auto-Stop, the other with Action=Auto-Start.
IAM Role

Create a Lambda execution role with AmazonEC2FullAccess.
Lambda Function

Please Check related python script in code section

Manually invoke the Lambda function.
