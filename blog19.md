# Creating a Billing Alarm to Monitor Your Estimated AWS Charges

Our senior project unfortunately left my account suspended after the Lambda functions which stopped the ec2 instances ceased to function. 

The billing console stated that I had missed two 130$ charges after failing to stop large ec2 instances

## Cloudwatch alarm creation

![image](https://user-images.githubusercontent.com/20525440/77815956-307d6000-707c-11ea-8b0c-f22a38f33a5c.png)

Cloudwatch can be used to create alarms that watch for specific events in various AWS services. Here we see the GUI for the alarm creation wizard.

![image](https://user-images.githubusercontent.com/20525440/77816087-1b550100-707d-11ea-90d0-6865f3fee98b.png)

We select a metric for cloudwatch to monitor on

![image](https://user-images.githubusercontent.com/20525440/77816096-2a3bb380-707d-11ea-8a2f-acc61a3bd03a.png)

You may need to switch to North Virginia in order to see the billing metrics. This is the region where billing metrics are held.

![image](https://user-images.githubusercontent.com/20525440/77816232-5e63a400-707e-11ea-8429-a058cf1d15ce.png)

We monitor for total estimated usage.

![image](https://user-images.githubusercontent.com/20525440/77816406-b64eda80-707f-11ea-85d6-b9c9fc060adb.png)

The conditions  for the trigger is if the estimated total charge of our monthly payments is above 50$ USD


![image](https://user-images.githubusercontent.com/20525440/77816448-eb5b2d00-707f-11ea-8d63-136c7ca8e2f8.png)


An AWS SNS topic is made in order to monitor the alarms created. Updates to this SNS topic will be sent out via email.

