# Blog 9 [11.9.2019]

The second week of our foray into infrastructure as code introduces us to another devops buzzword: terraform.

### Terraform by hashicorp
![image](https://banner2.cleanpng.com/20180529/szy/kisspng-terraform-hashicorp-microsoft-azure-infrastructure-5b0e0b6cc80963.2449977615276470848194.jpg)


Terraform is an application that seems to be the poster child of the "infrastructure as code" phenomenon.

Through various configuration files that are structured it's own native syntax, we can produce code to spin up machines that are exactly identical to each other, as well as change the amount of resources they individually use.

Terraform syntax is very similar to JSON, so much so they they even natively support JSON, as shown here:[https://www.terraform.io/docs/configuration/syntax-json.html ]

The only change being that the file must be named ###.tf.json as opposed to simply ###.tf

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
