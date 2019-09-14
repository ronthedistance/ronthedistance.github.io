# Blog 1 [9.13.2019]
### Week 2
```NOTE: This week I did not take screenshots throughout the process of completing the lab.```

###Highlights of this week

--Started playing with Jekyll, this is much better

---Did the lab

---Internship going well

---Finally having some ideas about a senior design project


### Switching to Jekyll
![jekyll logo](https://user-images.githubusercontent.com/20525440/64903453-147d1880-d66e-11e9-8f4f-e865b5fd1b51.png)

I don't know why I was so hardheaded about trying to do things through raw html, css, and what have you.
I had some javascript I wanted to test at some point but then realized github pages specifically only hosts static webpages.
Sooooo, no point in doing that.
Finally got off my lazy butt and decided to try Jekyll.
This is much easier and I regret being stubborn.
If you look through the messy commit history of this repo, you'll see my attempt at incorporating a better directory structure to the blog site. 
I'm unsure why they aren't simply working so that I could do a <base-url>/blog/blog<number> , but i'm sure i'm just missing some small detail that I'll facepalm myself for later.

***as a side note, because I kept refreshing the my browser after every commit, I was also manually refreshing my cache every time.
This was annoying.

I learned ctrl+f5 will reload a page without caching, making it much easier.

You might call it a messy use of the master branch, but I just refer to it as  ***atomic commits***
  
### Did lab 1. . . sort of.
  ![shruggo](https://user-images.githubusercontent.com/20525440/64903454-2ced3300-d66e-11e9-801c-75bac81ea24e.png)

  So Lab 1 basically is comprising of setting up a web application in a docker container.
  A dockerfile is used so that an image can be built. This makes it so multiple containers will run the exact same application using the same dependencies, file structure, networking, etc. Microservices for the win.
The dockerfile mainly composes of 

```RUN```which runs a specific binary upon building your image
and
```COPY```which copies a file into a specified directory/filepath within your container

both of these make up the sections which install packages such as apache or php, and resolves the dependencies necessary to operate them.

I also have 
```EXPOSE``` which exposes a port on build of your image
and
```CMD``` which can be used to run a command with specific parameters. 
  
these are the last two lines in the dockerfile and are part of an ongoing process to troubleshoot starting apache on starting the container.
Right now, everything seems to work fine IF you enter the container yourself and start it up via ```service apache2 start``` or ```service apache2 restart```
However, that's inefficient.

Some google tells us that using those commands in the dockerfile performs them as background processes, and in order to start them properly upon entering the container, apachectl needs to be run IN THE FOREGROUND.
However, this process of apache-on-startup is still ongoing. I cannot get it to work properly as of yet.

### Overexcited about new internship

I started an internship with the walt disney company this week! Don't want to post it to linkedin quite yet, but here's github hearing it first!

### Deciding between projects...
![Eureka](https://user-images.githubusercontent.com/20525440/64903522-96217600-d66f-11e9-9d37-e8a1632b7a17.jpg)

Obviously I will take this up with the group.

My two ideas are thus:



```Project OpenHive```


![openhive](https://user-images.githubusercontent.com/20525440/64903524-9752a300-d66f-11e9-90e9-1689b3a7f204.png)

An AWS / Docker based honeynet which will check hashes file uploads or attempted exploits in various databases like virus total or against MITRE CVEs.

Any which are not found in aforementioned databases will be kept in our own database for users to access and compare against traffic in their own networks.

It's kind of like some weird ... opensource....threat intel feed? I'm still not sure how / what to consider it.

```CTdeFense```


![defense](https://user-images.githubusercontent.com/20525440/64903509-6b372200-d66f-11e9-823d-ce3e6817fa0f.png)

Basically, CTFs are kind of offensively based in nature. I had the idea after Wicked6 to make a public CTF-ish site similar to overthewire, which would go over security basics in Linux.
Basically, you'd be given a dockerfile or access to an ssh server which you would have to connect to in order to complete a challenge.

For example, the first lab challenge might be: 
```"change the hashing algorithm from MD5 to SHA5 for your passwords in this machine. reset the root password to SomethingStrongerThanTheOriginalPassword$!$@```

Then, there would be a button on the site, or a script you run in the terminal like /lab1/submit/startattack
This would start up another container in that machine which would act as an offense machine.
If it is able to correctly break the root password, you don't get points.
If it sees that you correctly changed the passwords and the hashing algorithm, you get access to the next lab.

Of course this could be fleshed out better, but I feel like I'd rather at least have rough ideas as opposed to none.
