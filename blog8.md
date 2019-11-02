
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

