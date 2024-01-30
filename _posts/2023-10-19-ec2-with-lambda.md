---
layout: post
title: "Automating EC2 Operations with AWS Lambda: Starting an Instance and Running a Script on Linux"
categories: aws
---

AWS Lambda, a serverless compute service, provides a powerful way to automate tasks in AWS environments, including managing EC2 instances. In this guide, we'll explore how to use AWS Lambda to start a Linux EC2 instance and run a Python script automatically.

## Prerequisites
Before diving into the setup, ensure you have the following:
- An AWS account with access to EC2 and Lambda services.
- A Linux-based EC2 instance with the AWS Systems Manager (SSM) Agent installed. Modern Amazon Linux AMIs come with the SSM Agent pre-installed.
- An IAM role for the EC2 instance with the AmazonSSMManagedInstanceCore policy attached, allowing it to communicate with Systems Manager.
- An IAM role for the Lambda function with AmazonEC2FullAccess and AmazonSSMFullAccess permissions.

### Step 1: Prepare Your EC2 Instance
Ensure your EC2 instance is set up with the SSM Agent running and has the necessary IAM role attached. This setup is crucial for allowing Lambda to interact with the instance via Systems Manager without needing to open SSH ports.

### Step 2: Create Your Lambda Function
Navigate to the AWS Lambda console, and create a new function. Choose "Author from scratch," and select a runtime environment compatible with Python (e.g., Python 3.8). Attach the IAM role with EC2 and SSM permissions to your Lambda function.

### Step 3: Lambda Function Code
Here's the Python code for your Lambda function. This function starts your EC2 instance and then executes a command to run a Python script located in a specific directory on your instance.

```
import boto3

def lambda_handler(event, context):
    ec2_client = boto3.client('ec2', region_name='your-region')
    ssm_client = boto3.client('ssm', region_name='your-region')
    
    instance_id = 'your-instance-id'  # Replace with your instance ID
    
    # Start the EC2 instance
    ec2_client.start_instances(InstanceIds=[instance_id])
    print(f"Started EC2 instance: {instance_id}")
    
    # Wait for the instance to be running
    waiter = ec2_client.get_waiter('instance_running')
    waiter.wait(InstanceIds=[instance_id])
    print(f"Instance {instance_id} is running.")
    
    # Command to run your Python script
    command = 'cd /path/to/your/directory && python3 your_script.py'
    
    # Send the command via Systems Manager
    response = ssm_client.send_command(
        InstanceIds=[instance_id],
        DocumentName="AWS-RunShellScript",
        Parameters={'commands': [command]}
    )
    
    print(f"Sent command to run script, command ID: {response['Command']['CommandId']}")
    
    return {
        'statusCode': 200,
        'body': "Command to start Python script executed successfully."
    }
```
Replace 'your-region', 'your-instance-id', '/path/to/your/directory', and 'your_script.py' with your actual details.

### Step 4: Deploy and Test Your Lambda Function
After inputting the code, deploy your Lambda function. You can test it by creating a test event in the Lambda console and executing the function. If everything is set up correctly, your EC2 instance will start, and your Python script will execute automatically.