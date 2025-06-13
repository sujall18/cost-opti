# cost-opti

# AWS Lambda: Unused EBS Snapshots Cleanup

This AWS Lambda function automatically deletes unused or orphaned EBS snapshots to optimize storage costs.

##  Description

EBS snapshots in AWS can pile up over time, especially if volumes or instances are terminated. This Lambda function:

- Fetches all snapshots owned by your AWS account.
- Checks if the snapshot is associated with any volume.
- If the volume does not exist or is not attached to any running EC2 instance, the snapshot is deleted.
- If the snapshot is not associated with any volume at all, it's directly deleted.

##  How It Works

1. **List Snapshots**  
   Uses `describe_snapshots` to get all snapshots owned by the account.

2. **Check Active Instances**  
   Uses `describe_instances` to fetch all running EC2 instances.

3. **Validate Snapshots**  
   - If a snapshot has no associated volume  delete.
   - If the volume exists but has no active attachment  delete.
   - If the volume doesn't exist anymore  delete.

4. **Deletion**  
   Deletes unused snapshots using `delete_snapshot` API.

##  Code

The function is written in **Python 3.x** and uses **boto3** (AWS SDK for Python).

##  Deployment

1. Create an AWS Lambda function with Python runtime.
2. Attach the following AWS IAM permissions to Lambda's execution role:

```json
{
    "Effect": "Allow",
    "Action": [
        "ec2:DescribeSnapshots",
        "ec2:DescribeInstances",
        "ec2:DescribeVolumes",
        "ec2:DeleteSnapshot"
    ],
    "Resource": "*"
}

