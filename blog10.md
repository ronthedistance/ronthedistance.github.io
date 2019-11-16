# Blog 10 [11.15.2019]

With the terraform for my portion of th project completed last week,  I wanted to talk about a pet project I'm setting up to practice using AWS and/or terraform. 

## Custom Modded Minecraft servers via AWS

The idea to do this came about when I realized there aren't cheap cloud hosted services for modded minecraft servers.

The officially supported cloud solution for minecraft is "realms"
![image](https://user-images.githubusercontent.com/20525440/68989404-29576300-07fb-11ea-8026-793f7cb007ab.png)

However, realms only allows for a limited amount of users, 10. Additionally you cannot use mods on these servers, and only  the base "vanila" install will work.

![image](https://user-images.githubusercontent.com/20525440/68989388-eb5a3f00-07fa-11ea-9369-f77741db486f.png)

Looking online at prices for modded minecraft servers seems to be much pricier. The most popular solution "curseforge" is somewhat pricy. 

For a 6gb instance, you'd be paying normally more than 50$ per month.

The reason being that a modded minecraft server has much more dependencies to manage. For something like rlCraft (the mod I wanted to host) ```there are dozens of individual java files necessary to run a server.``` As shown below.

![image](https://user-images.githubusercontent.com/20525440/68989459-0f6a5000-07fc-11ea-8723-fa32009b5de6.png)

Each of these will have a minimum of 1 configuration file associated with it, and may have dependencies like a certain version of the java development kit or certain resource packs such as textures or shaders to be included in a readable directory.

Terraform is an application that seems to be the poster child of the "infrastructure as code" phenomenon.

Through various configuration files that are structured it's own native syntax, we can produce code to spin up machines that are exactly identical to each other, as well as change the amount of resources they individually use.

Terraform syntax is very similar to JSON, so much so they they even natively support JSON, as shown [here.](https://www.terraform.io/docs/configuration/syntax-json.html)

The only change being that the file must be named ```.tf.json``` as opposed to simply ```.tf```

That being said:on the topic of .tf files...



