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

As much as I appreciate how much work that is, I am NOT about to pay $50 a month for a gaming server, and I don't want to punch a hole in my home firewall for port forwarding to roll one off my home computer.

I also wouldn't be very fond of the electricity bill associated with this.


Thus I had my requirements:
```
-A publically accessible modded rlCraft minecraft server
-SPOT instancing instead of always up instacing to save money
-A way to automatically shut down / start up the server in order to additionally save money
-Dependencies managed automatically
-A way to change the modpack necessary in addition to saving world files
```




