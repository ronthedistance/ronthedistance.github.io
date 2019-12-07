# Blog [12.6.2019]
And with the end of the year, we end on a high note with a good amount of our final project working
## Amazon Elastic Container Service

![image](https://user-images.githubusercontent.com/20525440/70370496-b337a080-187c-11ea-9425-842fbabdf956.png)

Somewhat of an orchestrated containerzation process, ECS leverages multiple EC2 instaces that can distribute workload efficiently. The typical use case is clustering multiple ec2 instances that can act as docker hosts.

Whereas an ec2 instance is esssentially a virtual machine, i.e. a single raspberry pi, the ECS is multiple clustered raspberry pis, which can all act as docker hosts which spin up and down depending on application load.

## Task Definitions

![image](https://user-images.githubusercontent.com/20525440/70370552-54265b80-187d-11ea-897a-fdf00a433705.png)


The image above shows a basic diagram of how ECS objects relate to each other.

The cluster config, as mentioned before describes the HOSTS of the containers. These are ec2 instances running docker.

We'll table talking about services for now to mention task definitions.

Task definitions specifically work to define the containers inside your hosts.

These are small blueprints for your application, and describe containers through attributes, which can be easily visualized in terraform.


![image](https://user-images.githubusercontent.com/20525440/70370594-cd25b300-187d-11ea-8750-e534a934625a.png)

The code seen above is from the original test terraform following walkthroughs on youtube and github.

We see that the containers have port 80 open, use a certain amount of memory and cpu, use the latest version of the apache image on runtime, and have logDriver resources within the EU west availability. 

## Service Definitions

Referring to the same figure as the previous section, the "service" definition is meant to describe the clustered service that you are running.

Some of the highlights to point out in this section are 
- "desired_count" -- This states how many ec2 hosts are the "desired" state of your cluster. i.e. The service you run wants to get to a certain level of resources.It can scale up or maybe even down via an autoscaling policy, but it always wants to return to the number of resources required to run the desired count.

- load_balancer -- a load balancer is required to send requests to multiple containers via a round-robin priority system.

## Cluster
![image](https://user-images.githubusercontent.com/20525440/70370688-24785300-187f-11ea-9813-b2066ab9f7f1.png) 

Here is the launch configuration for the cluster in the 1c availabiulity zone.
Typically, when referring to a cluster via ECS, you're thinking about a 'Fargate' Cluster.

Fargate clusters run via individual ec2 instances that will automatically manage our containers for us. Here we see that it pulls the latest amazon linux AMI to build the ec2 instance, our variable for instance type in this case represents a t2.large ec2 instance to host the containers defined previously.

User_data is an attribute that defines what runs at startup of an ec2 instance in a fargate cluster.


![image](https://user-images.githubusercontent.com/20525440/70370727-9fda0480-187f-11ea-85f4-cf39780d22f0.png)

In our case, it simply makes sure that the right ECS cluster service is referenced upon startup of an ec2 instance.


## Elastic Container Registry

If you noticed in the pictures previous, there was a reference to an image named "terrawinchell:pleasework"

![image](https://user-images.githubusercontent.com/20525440/70370746-e92a5400-187f-11ea-8964-f50c02c65fe7.png)

The above picture shows that repository in question.

![image](https://user-images.githubusercontent.com/20525440/70370758-21ca2d80-1880-11ea-8332-0d5512e858cf.png)


After building a dockerfile, we can tag the image and push it to a remote repository that exists within our AWS console. 

Anything within the same region (us-west-1 for instance) will be able to reference
```464696867679.dkr.ecr.us-west-1.amazonaws.com/terrawinchell:latest```
in order to pull the image and use it to build a container.


Please see https://github.com/ronthedistance/ECS-services-DIPP for a working sample of an aws ECS cluster that serves a web application.
