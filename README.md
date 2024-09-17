#COST OPTIMIZATION
In this project, I developed a Lambda function to automate cost optimization for AWS by identifying and deleting unused EBS snapshots. Snapshots, which are backups of Amazon Elastic Block Store (EBS) volumes, can accumulate over time and incur significant storage costs if not actively managed.

The Lambda function:

Retrieves all existing snapshots within the AWS account.
Identifies whether each snapshot is attached to a volume that is associated with an active (running) EC2 instance.
Automatically deletes snapshots that are either not associated with any volume or linked to volumes that no longer exist, ensuring cost savings by eliminating unnecessary storage costs.
This solution leverages boto3, the AWS SDK for Python, to interact with AWS services like EC2 and implement snapshot lifecycle management.

Key Highlights:
Cost Efficiency: Optimized AWS spending by cleaning up unused snapshots.
Automation: Fully automated snapshot cleanup process, reducing manual overhead.
AWS Services: Utilized AWS Lambda, EC2, and EBS to implement the solution.

 Steps for AWS Cost Optimization Using Lambda:

1. Set Up an AWS Lambda Function:
   - Go to the AWS Management Console.
   - Navigate to AWS Lambda.
   - Create a new Lambda function using Python as the runtime.

2. Write the Lambda Code:
   - Use the Python code you created to:
     - List all EBS snapshots in your AWS account.
     - Check whether the snapshots are linked to any running EC2 instances.
     - Delete any unused snapshots (those that are not attached to active EC2 volumes).

3. Create an IAM Role for Lambda:
   - Go to IAM in the AWS console.
   - Create a new role, and choose Lambda as the trusted service.
   - Attach a custom IAM policy with the following permissions:
     - `ec2:DescribeSnapshots` (to list snapshots).
     - `ec2:DescribeInstances` (to check running instances).
     - `ec2:DescribeVolumes` (to check volume details).
     - `ec2:DeleteSnapshot` (to delete unused snapshots).
   - Attach this role to your Lambda function so it has the required permissions.

4. Test Your Lambda Function:
   - Trigger the Lambda function manually from the AWS console to ensure it works as expected.
   - Review the logs in **CloudWatch** to verify that snapshots are being deleted as intended.

5. Set Up CloudWatch EventBridge (Scheduled Trigger): (optinal)
   - To automate the process, navigate to Amazon EventBridge (previously known as CloudWatch Events) in the AWS console.
   - Create a new rule for a scheduled event:
     - Choose the option to schedule an event.
     - Set the frequency (e.g., daily or weekly) using **cron** or **rate** expressions.
     - Example cron expression for daily at midnight: `cron(0 0 * * ? *)`.
   - In the 'target' section, select 'Lambda function' and choose the Lambda function you created.
   - This schedule will trigger your Lambda function automatically at the specified time, regularly cleaning up unused snapshots.

6. Monitor:
   - Use CloudWatch Logs to monitor the function’s execution and ensure only the intended snapshots are being deleted.
   - Adjust the frequency of the EventBridge rule or Lambda function logic as necessary based on performance and results.

- Lambda function handles the actual logic of identifying and deleting unused EBS snapshots.
- CloudWatch EventBridge is used to automatically trigger the Lambda function periodically, ensuring regular snapshot cleanup.
  
This automation helps you reduce AWS costs by preventing unnecessary EBS snapshot accumulation, without requiring manual intervention.
