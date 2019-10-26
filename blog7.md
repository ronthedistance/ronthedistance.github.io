# Blog 6 [10.18.2019]
### Week 5

So it turns out I completely did the VPC and Subnetting incorrectly last time.

Today is just going to be some screenshots on how I'm pretty sure it's supposed to look. 
Or maybe I was just confused about requirements 

### Creating the VPC (Again...)


So. I don't exactly remember the specifics, but I remember saying to myself "oh. I did it all wrong. To sate my paranoia, the requirements are shown here.

-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-
![image](https://user-images.githubusercontent.com/20525440/67138822-eefba580-f1fd-11e9-9e4b-66782e848cf5.png)
-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-
### Creating the subnets
https://www.aelius.com/njh/subnet_sheet.html
I used this cheatsheet to reference the subnet IDs this time.

The first 3 subnets are public with 1024 available IP addresses.

You also see the available number of hosts in column B.

Column C highlights the different availability zones.

Column D highlights the routing table.

![image](https://user-images.githubusercontent.com/20525440/67138982-9fb67480-f1ff-11e9-89b9-cea432d68c9d.png)


![image](https://user-images.githubusercontent.com/20525440/67139001-bd83d980-f1ff-11e9-9e19-4819f136947f.png)

The subnets are created via the subnet creation tool seen as "Create Subnet", the royal blue button at the top left of the subnets area.

Name tag here corresponds to column A in the first figure.
The VPC being used is obviously the non-default one that was created.
The availability zone corresponds to column D in the first figure.
The IPv4 CIDR block is used to determine the range and number of hosts within this subnet, see subnet cheat sheet shown before.

```The process for public and private subnets was majority the same, the different being the number of hosts being specified by a /20 network as opposed to a /22```


### Creating the routing table(s)

![image](https://user-images.githubusercontent.com/20525440/67139159-ba89e880-f201-11e9-94af-7b765841df15.png)

The routing table page looks like this. Here we see the 2 default subnets in addition to the 2 created by our team.


![image](https://user-images.githubusercontent.com/20525440/67139193-2bc99b80-f202-11e9-9541-90f3aec3e3ad.png)

Changing the routes within rquires you to simply click the route you want to edit and click "edit routes"

shown is the routing table used for the "public" route.


### Internet Gateway
![image](https://user-images.githubusercontent.com/20525440/67139217-574c8600-f202-11e9-9b85-63e0733a5e20.png)

The current IGW looks like this. This feature acts like a virtual router that connects us to public internet.

In the previously shown routing table, we see that our IGW is connected to the public routing table. 

With that we have sufficiently completed all requirements.

