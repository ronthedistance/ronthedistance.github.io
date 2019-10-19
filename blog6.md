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

