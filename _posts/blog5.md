# Blog 5 [10.10.2019]
### Week 4

With this week comes the class' first official foray into AWS.
The first project not only introduced me to those cloud concepts but one thing I'll talk about a bit first.

### Github
So, I'm not going to lie. I'm not as well versed in github aside from commits and pushes. 
I've never had to manage the actual repositories.

```Issues``` 

We haven't used issues in our project as of yet, but through them we can assign various tasks to our group members.
It also allows you to label these issues, allowing others who visit the repository to see bugs, documentation fixes, and the like.

```Wiki```

This section is self explanatory, but it's nice to know that I was missing out on an entirely helpful section of github pages because i really only went so far as the directories I was looking for once I found the pages I wanted in google.

```Projects```

Oh. A built-in github kanban board? I'm unsure if I wanted to use this over the issues feature, but I think that for now, the kanban board is easier to follow. The biggest downside is that visibility seems to be limited to those within the actual repository. ...I think.


### First shot at AWS...
![image](https://user-images.githubusercontent.com/20525440/66695267-649ec900-ec74-11e9-814f-13831121a5db.png)

So, my assignment for this project is handling the VPC. This is like having a private address range that you can use for your instances.

The first roadblock is currently carving out the IP range.
Project requirements state 3 public  ip ranges of 1024 hosts, and 3 private ip ranges of 4096 hosts each.
This is 3072 + 12288 hosts, meaning we'd need around 16000 ip addresses, with a total of 6 subnets. 
Wanting to save space, I carve out a 10.0.0.0/18, which has a max 16384.

![image](https://user-images.githubusercontent.com/20525440/66695618-b2b5cb80-ec78-11e9-88fb-50c76d477547.png)


We then further drill down into /22 for the public subnets within the vpc, and /20 for the private subnets.
Upon using the VPC wizard, the route tables and NAT instances were pre-configured.
``` See this link for more specific details:  ```https://docs.aws.amazon.com/vpc/latest/userguide/getting-started-ipv4.html

However, this was not everything that needed to be done.
For some reason, we cannot SSH into the ec2 instance that resides within the VPC via ssh. 
https://aws.amazon.com/premiumsupport/knowledge-center/ec2-linux-ssh-troubleshooting/

The troubleshooting guide shown above states that this may be due to not having a default route that points to our internet gateway. 
I currently type this from a work laptop, and will update this setting in our management console as soon as I get home.

For now: know that while it only took me a single page to explain the subnet setup, it took much more time at first to properly subnet the network, mainly because I forgot that subnet calculators existed.
