# Web Application Firewalls

## Why?

Because....I don't know. I was messing with Apache for Ansible playbooks and wanted to see what other modules we could put to make it a more stable server.

I realized putting a WAF in front of it might be helpful in case it was placed in a dubious DMZ of some sort. 

## What are they?

While a typical firewall blocks based on network-level rules (ports, ips, connection states), a web application firewall is a layer 7 defense (with the application being HTTP)

By reading the desired payload or requests, we can block attempts at various attacks such as SQL injections, cross site scripting, cross site request forgery , etc.

WAFs can do this by employing a reverse proxy, which forces clients to send traffic to the WAF before reaching the resource  server.

The WAF is then configured with policies designed to perform certain actions based on specific information given in a POST request or other request to the web server. 

An example is shown here: 

![image](https://user-images.githubusercontent.com/20525440/80272870-2ef68600-8682-11ea-9f2d-1d5387ab0b74.png)

Here, we see that StackPath's configuration is preventing POST requests to the /pictures/upload url path by instead giving out a 403 error
