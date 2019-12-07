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
