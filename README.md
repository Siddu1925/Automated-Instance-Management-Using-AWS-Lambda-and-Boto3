# Automated-Instance-Management-Using-AWS-Lambda-and-Boto3

Objective : Automates the Start and Stop instances states based on the individual tags assigned using AWS Lambda function and Boto3.

Steps:
1.EC2 Instance setup
  Create two EC2 instances.
Tag one with Action=Auto-Stop, the other with Action=Auto-Start.
IAM Role

2.Create a Lambda execution role with AmazonEC2FullAccess.
Lambda Function
# Lambda function to start/stop EC2 instances based on tags
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    
    # Find instances with Action=Auto-Stop
    stop_response = ec2.describe_instances(
        Filters=[
            {'Name': 'tag:Action', 'Values': ['Auto-Stop']},
            {'Name': 'instance-state-name', 'Values': ['running', 'pending']}
        ]
    )
    stop_ids = [
        instance['InstanceId']
        for reservation in stop_response['Reservations']
        for instance in reservation['Instances']
    ]
    
    # Find instances with Action=Auto-Start
    start_response = ec2.describe_instances(
        Filters=[
            {'Name': 'tag:Action', 'Values': ['Auto-Start']},
            {'Name': 'instance-state-name', 'Values': ['stopped']}
        ]
    )
    start_ids = [
        instance['InstanceId']
        for reservation in start_response['Reservations']
        for instance in reservation['Instances']
    ]
    
    # Stop instances
    if stop_ids:
        ec2.stop_instances(InstanceIds=stop_ids)
        print(f"Stopped instances: {stop_ids}")
    else:
        print("No instances to stop.")
    
    # Start instances
    if start_ids:
        ec2.start_instances(InstanceIds=start_ids)
        print(f"Started instances: {start_ids}")
    else:
        print("No instances to start.")

    return{
        'StoppedInstances': stop_ids,
        'StartedInstances': start_ids
    }
    
3.Manually invoke the Lambda function.
