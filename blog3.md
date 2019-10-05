# Blog 4 [10.4.2019]
### Week 5

As is iterated from previous weeks, I am really good at procrastinating.
Thus, project on the AWS project has yet to be made
As the appointed leader of the group I should be leading by example though... 

### Hyper-V issues at work
![image](https://user-images.githubusercontent.com/20525440/66249383-4cb0cd80-e6e7-11e9-8219-7f3d8f12b6b5.png)

While I don't have anything to talk about AWS-wise for the senior project, I hope that this random github post helps someone else in the future.
Hyper-V is a built in hypervisor, basically used to manage virtual machines.I run various configurations of kali and onion VMs in order to validate qualys data about vulnerabilities.
```But why make this post about all that?```

I don't remember when it happened specifically, but at some point in the day I ran into the following error:


### Hyper-V Error Code 32788

Looking on google tells us this error is meant to be related to a problem with your virtual switch configuration. 
![image](https://user-images.githubusercontent.com/20525440/66249444-57b82d80-e6e8-11e9-94bd-5a8646bcdc30.png)
```picture taken from google```
![image](https://user-images.githubusercontent.com/20525440/66249547-f85b1d00-e6e9-11e9-94bd-51be55059915.png)
```picture taken on official microsoft forums```

*I feel like at that much it's safe to assume that's the answer...*
So I went through multiple different iterations of virtual switches. External, internal, bridged, host only, and even tried NIC bonding. No dice.
I then made a new VM, with the exact same VDK (basically a backup copy of the DATA of the virtual machine)

```and everything works?...```

### What gives?

To save you all a long story short of being that annoying intern that couldn't figure out how to make a virtual machine---
See the following image.
![image](https://user-images.githubusercontent.com/20525440/66249600-e29a2780-e6ea-11e9-85d4-1052f8ded73e.png)

VM limits in Hyper-v state that from a technical standpoint, it should be able to reach terabytes of RAM if you're able to allow it.
HOWEVER.
That is NOT the hyper-v that comes default to your computer.

![image](https://user-images.githubusercontent.com/20525440/66249632-6b18c800-e6eb-11e9-97d1-875904929c44.png)


```by default, the standalone version of Hyper-V can allot 4096 bits of memory to a VM before throwing errors```
### The Solution

After changing the amount of memory being used by the original VM, it succuessfully booted up, giving me something to write about tonight!!!
