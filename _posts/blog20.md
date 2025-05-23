# Ghidra SRE

![image](https://user-images.githubusercontent.com/20525440/78420301-9cbf0d00-7602-11ea-912e-df1ba7db16dd.png)


## What is Ghidra?

Ghidra is a software reverse engineering application developed by the NSA, with it, we can take a lower level look at compiled executables / binaries to see what's there "under the hood"

## Starting

![image](https://user-images.githubusercontent.com/20525440/78419237-ad6a8580-75f8-11ea-858a-cab72e0d2130.png)

Upon proper installation of the application from ghidra-sre.org, we see a ghidraRun file. The .bat file under it is for Windows.

![image](https://user-images.githubusercontent.com/20525440/78419460-c2481880-75fa-11ea-97ed-ee3d39ff5a36.png)

Normally this window would be empty, but this shows your available projects to use. This is basically a set of binaries to analyze and the plugin settings you have while analyzing them.

A new project can be made like so : 

![image](https://user-images.githubusercontent.com/20525440/78419625-6aaaac80-75fc-11ea-910d-5ffa23ad7ef9.png)

##Analysis basics

![image](https://user-images.githubusercontent.com/20525440/78419642-87df7b00-75fc-11ea-98e5-64ebc78d73bd.png)

In the project, we can then import the file we want to analyze.

![image](https://user-images.githubusercontent.com/20525440/78419712-5e731f00-75fd-11ea-8808-4c0ca6f1cd4a.png)

Most likely, you'll get a window similar to this upon start. click yes.

![image](https://user-images.githubusercontent.com/20525440/78419723-75197600-75fd-11ea-81ba-3ec857214987.png)

These are the default options for ghidra to scan the executable for. Most likely you don't need anything past the default.



![image](https://user-images.githubusercontent.com/20525440/78419835-62537100-75fe-11ea-9846-7d3a30e3d7bc.png)


After scanning, some windows may pop up. Currently I have the Symbol Tree, Program Tree, Data Type Manager, Disassembled View, Listing, and Decompiled view open.

![image](https://user-images.githubusercontent.com/20525440/78419859-a0e92b80-75fe-11ea-90aa-e172c62ee06e.png)


If none of the mentioned windows show up, we can use the "windows" drop down menu at the top.

![image](https://user-images.githubusercontent.com/20525440/78419882-ead21180-75fe-11ea-95f5-465787318363.png)

Another interesting window to look at is the function call graph, which creates a visual representation of subroutine and any additional function calls made within the function you are currently looking at in the symbol tree.

In the above, we are currently in the _start function label, which calls a bunch of libraries in addition to a "main" function. We can then look at "main" in the symbol tree

![image](https://user-images.githubusercontent.com/20525440/78420232-1a364d80-7602-11ea-923d-e2edcd3273e3.png)

We can also infer this from the disassembled / decompiled code.

![image](https://user-images.githubusercontent.com/20525440/78420321-d5f77d00-7602-11ea-82a0-36e4ceaef5c9.png)

From the decompiled view, we see that the binary file we have is a file that loops until the hex value 28, AKA the decimal value 40, then exists.
