---
title: "Testing & Validation Automation"
date: 2025-08-09T00:00:00+07:00
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

**Content:**
- [Create Lambda Validation Function](#create-lambda-validation-function)
- [Create Validation EventBridge Rule](#create-validation-eventbridge-rule)

---

### Create Lambda Validation Function

#### Access Lambda Console
1. Search for **"Lambda"** in the AWS Console search box.  
2. Click **Create function**.

![Monitor Task Status](/images/5-/lamb1.png)
![Monitor Task Status](/images/5-/lamb2.png)

#### Function Configuration
Function name: migration-validator
Runtime: Python 3.11
Architecture: x86_64



![Monitor Task Status](/images/5-/lamb3.png)

#### Lambda Function Code
```python
import json
import boto3
from datetime import datetime, timedelta

def lambda_handler(event, context):
    """Monitor DMS task instead of direct database validation"""
    
    dms = boto3.client('dms')
    cloudwatch = boto3.client('cloudwatch')
    
    results = {
        'timestamp': datetime.now().isoformat(),
        'validation_results': {}
    }
    
    try:
        # Get DMS task status
        task_response = dms.describe_replication_tasks(
            Filters=[
                {
                    'Name': 'replication-task-id',
                    'Values': [event.get('task_id', 'mysql-to-postgres-migration')]
                }
            ]
        )
        
        if task_response['ReplicationTasks']:
            task = task_response['ReplicationTasks'][0]
            
            results['task_status'] = task['Status']
            results['task_progress'] = task.get('ReplicationTaskStats', {})
            
            # Get CloudWatch metrics
            end_time = datetime.utcnow()
            start_time = end_time - timedelta(minutes=15)
            
            # CDC Latency
            latency_response = cloudwatch.get_metric_statistics(
                Namespace='AWS/DMS',
                MetricName='CDCLatencySource',
                Dimensions=[
                    {
                        'Name': 'ReplicationInstanceIdentifier',
                        'Value': event.get('replication_instance', 'migration-replication-instance')
                    }
                ],
                StartTime=start_time,
                EndTime=end_time, 
                Period=300,
                Statistics=['Average']
            )
            
            if latency_response['Datapoints']:
                latest_latency = sorted(latency_response['Datapoints'], 
                                      key=lambda x: x['Timestamp'])[-1]
                results['cdc_latency_seconds'] = latest_latency['Average']
                results['latency_status'] = 'OK' if latest_latency['Average'] < 300 else 'HIGH'
            
        results['status'] = 'success'
        
    except Exception as e:
        results['error'] = str(e)
        results['status'] = 'failed'
    
    return {
        'statusCode': 200,
        'body': json.dumps(results, default=str)
    }
```
Click Deploy.

![Monitor Task Status](/images/5-/lamb4.png)

### Create Validation EventBridge Rule
#### Access EventBridge Console
1. Search for "EventBridge" in the AWS Console search box.

2. Click Rules.


![Monitor Task Status](/images/5-/evb1.png)

#### Create Scheduled Rule
Click Create rule.

Rule name: migration-validation-schedule

Rule type: Schedule

Schedule pattern: Rate-based

Rate expression: rate(15 minutes)


![Monitor Task Status](/images/5-/evb2.png)

#### Add Target
Target type: AWS service

Service: Lambda function

Function: migration-validator

Configure input: Constant (JSON text)

JSON:

```json
{
  "source_host": "SOURCE-MYSQL-ENDPOINT",
  "source_user": "admin",
  "source_password": "MyPassword123!",
  "source_database": "sampledb",
  "target_host": "TARGET-POSTGRES-ENDPOINT", 
  "target_user": "postgres",
  "target_password": "MyPassword123!",
  "target_database": "targetdb"
}
```
Click Create rule.
