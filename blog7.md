# Blog 7 [10.25.2019]
### Week 5


### Memory forensics basics with Volatility

At my internship, there was an end of the year event that was a large assembly of capture-the-flag challenges surrounding what we did in the various functions. This post talks about how I got one of these flags by enumerating process within a windows dump file, and grepping for the information I desired.



From the beginning, I was given a .dmp file called ```refresh-my-memory.dmp```

These files are typically used to help determine why something crashes, akin to a windows blue screen making process information files upon crash. 

![image](https://user-images.githubusercontent.com/20525440/67614990-1cef6580-f77b-11e9-9731-c6c3e08ee90f.png)

You can also create a dump file of a specific process via task manager in windows.

More information on dump files shown here: https://fileinfo.com/extension/dmp

While there are tools you can use such as windows SDK to debug using this file, volatility is a popular memory forensics tool that you can use to parse the process information from these files.

More information on volatility shown here: https://www.volatilityfoundation.org/26


-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-
### Memory dumping
-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-

Before doing anything with the actual process information, we want to enumerate the information necessary to read the file. 
```
volatility -f  refresh-my-memory.dmp imageinfo
```
The command above is used to get information about the dmp file. 

![image](https://user-images.githubusercontent.com/20525440/67615010-8a02fb00-f77b-11e9-941c-2ee9cc0bed5e.png)

Notice the “Suggested Profile(s)” section. Profiles in volatility are used to specify what type of system the memory dump came from. Without a properly selected profile, we may not be able to find any proesses, or have mangled names such as: “?|?A[]??csrss.ex”.
We end up using the first profile, Win7SP1x64. It is the first format given to us and seems to work for our purposes.

The following command will search througuh process information within the dump file.

```volatility -f refresh-my-memory.dmp --profile=Win7SP1x64 pslist```

![image](https://user-images.githubusercontent.com/20525440/67615028-c2a2d480-f77b-11e9-9003-7f9e67a6bfb3.png)

PS list is a volatility modules which lists any information about processes in a given file.
Here we see the memory address, name, process id, thread count, and more. 
An alternative to pslist is psscan, which does the same thing but additionally attempts to locate information on terminated processes and "hidden" processes.


-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-
### Finding the actual flag(s)
-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-

![image](https://user-images.githubusercontent.com/20525440/67615054-0d245100-f77c-11e9-9be8-d0f62e5198e5.png)

After spot checking the numerous processes given off by pslist (this dump file was almost 3 gigabytes), we notice two instances of notepad open from the user.

Note that after "notepad.exe" we have a column specifying the PID of the process at the time the dump is taken. We can use this to more specifically find memory that we're interested in with the following command.
``` Volatility –f ./refresh-my-memory.dmp –profile Win7SP1x64 memdump –dump.dir=./FileHandles –p 328```

"memdump" is a volatility module that is short for "memory dump". as the name implies, it takes the whatever information can be found about a given processes' stack frame at the time of execution. 

From there the __-dump.dir__ specifies a directory to use to send the dump file to.

The __":-p"__ option specifies a process ID to use, which in our case was 328.

![image](https://user-images.githubusercontent.com/20525440/67615115-d7cc3300-f77c-11e9-9752-4cf416ec406a.png)

This gives us a new file containing the raw memory of that process in execution.

From here ,this was more about the CTF, but given that you had a goal in mind in real life, such as retaining portions of a document or trying to ssee if a malicious actor was sending emails to someone in the Czech republic, the following methodology might still apply.

As this was a capture the flag challenge, I knew the format of the answer I wanted to find.

Thus, we can use strings to parse through the readable portions of memory, and then parse that for the flag format.

![image](https://user-images.githubusercontent.com/20525440/67615140-34c7e900-f77d-11e9-8724-f5031a2e02f7.png)

And thus I found what I was looking for. 

![image](https://user-images.githubusercontent.com/20525440/67615145-401b1480-f77d-11e9-823f-d740b83f66f9.png)


-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+--+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-

Incidentally speaking,  I ended up finding another flag on accident whilst looking through the dump of a photoshop file.

While I don't have a screenshot, I later found out it was possible to retrieve the raw binary information of an image being used in photoshop to then import into image viewer in order to view it's contents.

Dump files are made on occassion, and if a malicious actor can crash your computer in the right way, they can attempt to later retrieve the dump file in order to find out what you were using your workstation for at the time.
