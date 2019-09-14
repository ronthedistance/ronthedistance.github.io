# Blog 1 [9.13.2019]
### Week 2
```NOTE: This week I didn't not take screenshots throughout the process of writing the lab.```

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
  
### Did lab 1...sort of.
  
  So Lab 1 basically is comprising of setting up a web application in a docker container.
  A dockerfile is used so that an image can be built. This makes it so multiple containers will run the exact same application using the same dependencies, file structure, networking, etc. Microservices for the win.
The dockerfile mainly composes of 
```RUN```which runs a specific binary upon building your image

and

```COPY```which copies a file into a specified directory/filepath within your container
  
