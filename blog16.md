# Blog 16 [2.21.2020]

## AWS Auto Remediate

![image](https://user-images.githubusercontent.com/20525440/75088298-1e9a2180-5500-11ea-80c1-707e267f7d7b.png)

I wanted to take the chance to highlight a tool I found this week. 

It uses the following resources:
- CloudFormation Stack 	
- CloudWatch Event Rule
- DynamoDB Table 	auto-remediate-settings
- Lambda Function 	
  - auto-remediate
	- auto-remediate-dlq
	- auto-remediate-setup
- SNS Topic 	
  - auto-remediate-log (not functional #19)
	- auto-remediate-missing-remediation
- SQS Queue 	
  - auto-remediate-config-compliance
	- auto-remediate-dlq
  
Basically, we can configure a "settings" dynamo DB table that tells the "auto remediate" functions what to do if certain types of configurations are seen in AWS.
This can be something like whether or not a relational Data store is publically accessible, or if SSL is not enforced in requests to your s3 bucket.

Cloudformation is used to set up the application architecture.
From then on, when certain Cloudwatch events are seen, an SQS queue is populated that is polled by the "auto remediate" Lambda function.

That "auto remediate" lambda function will additionally write to an SQS queue should it fail to remediate an issue, called the "DLQ"
There is a customizable "RETRYCOUNT" which prevents the function from infinitely using lambda resources should the function never be able to remediate the issue. 
Finally, logs about the function are written to an AWS SNS Topic to be inspected by an administrator on a weekly or monthly basis.






