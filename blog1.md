# Blog 1 [9.13.2019]
### Week 2
```NOTE: This week I did not take screenshots throughout the process of completing the lab.```

###Highlights of this week

--Started playing with Jekyll, this is much better
---Did this lab
---Internship going well

### Switching to Jekyll

I don't know why I was so hardheaded about trying to do things through raw html, css, and what have you.
I had some javascript I wanted to test at some point but then realized github pages specifically only hosts static webpages.
Sooooo, no point in doing that.
Finally got off my lazy butt and decided to try Jekyll.
This is much easier and I regret being stubborn.
If you look through the messy commit history of this repo, you'll see my attempt at incorporating a better directory structure to the blog site. 
I'm unsure why they aren't simply working so that I could do a <base-url>/blog/blog<number> , but i'm sure i'm just missing some small detail that I'll facepalm myself for later.
  
### Did lab 1....sort of.
  
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
  
these are the last two files in the dockerfile and are part of an ongoing process to troubleshoot starting apache on starting the container.
Right now, everything seems to work fine IF you enter the container yourself and start it up via ```service apache2 start``` or ```service apache2 restart```
However, that's inefficient.

Some google tells us that using those commands in the dockerfile performs them as background processes, and in order to start them properly upon entering the container, apachectl needs to be run IN THE FOREGROUND.
However, this process is still ongoing. I cannot get it to work properly as of yet.
