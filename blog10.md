# Blog 10 [11.15.2019]

With the terraform for my portion of the project completed last week,  I wanted to talk about a pet project I'm setting up to practice using AWS and/or terraform. 

## Custom Modded Minecraft servers via AWS

The idea to do this came about when I realized there aren't cheap cloud hosted services for modded minecraft servers.

The officially supported cloud solution for minecraft is "realms"
![image](https://user-images.githubusercontent.com/20525440/68989404-29576300-07fb-11ea-8026-793f7cb007ab.png)

However, realms only allows for a limited amount of users, 10. Additionally you cannot use mods on these servers, and only  the base "vanila" install will work.

![image](https://user-images.githubusercontent.com/20525440/68989388-eb5a3f00-07fa-11ea-9369-f77741db486f.png)

Looking online at prices for modded minecraft servers seems to be much pricier. The most popular solution "curseforge" is somewhat pricy. 

For a 6gb instance, you'd be paying normally more than 50$ per month. Note that 6 gb is a typical amount of RAM for this specific mod, due to how the world loads maps and how intensive the mob AI is.

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

###Progress so far...

![image](https://user-images.githubusercontent.com/20525440/68989571-abe12200-07fd-11ea-82c2-2641bf7661f2.png)

Luckily for me, there is a little bit of precedence regarding this matter. The service diagram shown above is what I'm going to be emulating for the skeleton of this project. Some eventual additions include some lambda functions which will webscrape the necessary webpages in order for me to figure out what the most recent mod versions are, what version of the modloader I'll need to download, and the ability to load different configuration files from an s3 bucket to change how the world works (e.g. less agressive mobs, different world seedfiles, etc).

Note that the port specified *25565* is the port necessary to run a publicly accessible minecraft server. I also wish to create a cloudwatch event that monitors traffic to my ec2 instance on this port. If there is no traffic to my ec2 instance on that port for, say, 2 hours, I want a lambda function to run to stop the instance so I'm not wasting precious money.

# s3 bucket
![image](https://user-images.githubusercontent.com/20525440/68989639-83a5f300-07fe-11ea-9da9-a1539fe6255d.png)

I started by creating an s3 bucket to contain any necessary files.

![image](https://user-images.githubusercontent.com/20525440/68989652-9b7d7700-07fe-11ea-9ade-d54a6625e41a.png)

This bucket currently contains a "setup" folder, which holds 3 things: The modloader, a systemd service file called minecraft.service, and a mods.zip folder which contains the necessary files for the server to run. Note that the modloader version is 1.12.2, build 2756. This is a very specific number necessary to run the current version of rlCraft. For interest's sake, the current version of minecraft is 1.14, heading into 1.15 early 2020.

![image](https://user-images.githubusercontent.com/20525440/68989727-6cb3d080-07ff-11ea-8e48-0a1101f381a6.png)
![image](https://user-images.githubusercontent.com/20525440/68989738-9240da00-07ff-11ea-8721-49e72b55b1cd.png)

Through the twitch desktop app, there does exist a pre-made modpack for rlcraft. exporting the profile and opening the folder associated shows up the mods necessary to run this version of the game.

For anyone reading this in the future, it may be useful to run this mod locally to see if you're comfortable lowering the amount of memory to 4 or 5 gb, or if you want even more memory . I am personally fine with how it runs on 6gb of ram.

We  can eventually webscrape this page for the necessary mod, modloader, minecraft version numbers we need. For now, we will zip the necessary mods we need and leave it as is.

![image](https://user-images.githubusercontent.com/20525440/68989767-07141400-0800-11ea-9c53-1531fcb06921.png)

The systemd "minecraft.service" file in the bucket allows us to start the minecraft server as soon as the ec2 instance turns on. Notice that the service type is ran by the user "ec2-user" and uses the java binary on the "forge-1.12.2-14.23.5.2838-universal.jar" which is our modloader.

#permissions and IAM

With the s3 bucket containing the resources necessary, we need to create a role in IAM that allows the minecraft server to both pull and save to the bucket in question ( named rlcraft-server-l8 in our example )

![image](https://user-images.githubusercontent.com/20525440/68989838-d5e81380-0800-11ea-9896-8224ad72b44b.png)

This is the policy created that will be attached to the role. Note the most important functions: "Listbucket, getobject, putobject".

![image](https://user-images.githubusercontent.com/20525440/68989848-f6b06900-0800-11ea-8569-67be3feea959.png)

This is the role which that policy attaches to. It's an ec2 instance role with specific s3 bucket permissions.
