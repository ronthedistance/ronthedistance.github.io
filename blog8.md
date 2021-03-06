
# Blog 8 [11.1.2019]

This week is our first practical foray into infrastructure as code. 

### AWS Lambda.
![image](https://user-images.githubusercontent.com/20525440/68066921-d52d8880-fcfc-11e9-8d8a-613aeb5028e7.png)


AWS lambda is important because it defines applications that don't need actual infrastructure to run.

Earlier in the semester we saw that ec2 instances can be used similar to cloud - based virtual machines.

AWS lambda can potentially make those obselete by being able to run the same application , but only getting billed when the code actually executes.

![image](https://user-images.githubusercontent.com/20525440/68067266-9b12b580-fd01-11e9-825e-2837c91c09b4.png)

The following image shows the current status of our start/stop ec2 instance lambda functions.

![image](https://user-images.githubusercontent.com/20525440/68067287-cc8b8100-fd01-11e9-80d2-f67ec3688221.png)

The image shown is the simple code necessary to complete the ec2 start in this case.
Using a python 3.7 environment, it creates an object that references our ec2 instance named "ec2". From there, the object has a built in function called start_instances that spins up the instance id given to it when called.

![image](https://user-images.githubusercontent.com/20525440/68067305-16746700-fd02-11e9-80a1-080479289226.png)

The above image is the results of testing the function.



![image](https://user-images.githubusercontent.com/20525440/68067327-7d921b80-fd02-11e9-86e0-2c89dac62439.png)


Cloud watch is another AWS service that allows you to monitor resources and applications. For our purposes however, we can use it to set rules to run the start ec2 instance function similar to a crontab.


![image](https://user-images.githubusercontent.com/20525440/68067359-f5f8dc80-fd02-11e9-967f-a77b02d75106.png)

Under cloudwatch --> rules --> create rules we can assign a crontab-like configuration to a target lambda function.

Here, we use the cron expression ```"0 3 ? * 6 *"```. 

Note that in order for this to function, either the day or the month or the day of the week portion of the cron expression MUST be a question mark. I had a lot of head aches with this. 

The question mark functions similarly to a wildcard, but exists because the number of days in a month can vary. By designating a question mark, it stands for "no specific value" as opposed to "any / all potential values" that a wildcard would stand for.

Our specific cron expression stands for:
```3:00 on any given day of the month that is a friday```

shown in the cron expression "next trigger dates" in the left panel.


The cloudwatch console is important because it allows us to schedule uptime for our instances specifically for the portions of time that we need them up to demo for senior design. 

This saves us precious AWS credits now that our AWS free tier has expired on our console, so long as we don't manually start services unecessarily.
