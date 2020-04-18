# blog 22 Ghidra SRE Con't -- Function Signatures

Decompilers and dissassemblers are useful, but not an end-all be all. 

It can't perfectly pull out all the parameters, comments, and return values for the binary you are analyzing, so sometimes you have to be locate those signatures online and replace them to get a full understanding of the code


### what are function signatures?

Function signature basicaly define the input and output of a function.

so for any example 

![image](https://user-images.githubusercontent.com/20525440/79630206-3b1f9800-8104-11ea-81d4-2ed3d92cb62f.png)

This binary refers to a very common windows function named winMain, but if we look at the function label given to us

![image](https://user-images.githubusercontent.com/20525440/79630216-5d191a80-8104-11ea-87c1-d35a9443f171.png)


It calls it as an undefined void returning function

Knowing beforehand that this is winmain, we can change the function signature so that the parameters aren't as confusing going forward. 

This is taken from the official microsoft documentation on the function.

![image](https://user-images.githubusercontent.com/20525440/79630280-dca6e980-8104-11ea-99b1-dea11f995b6e.png)


![image](https://user-images.githubusercontent.com/20525440/79630406-d7966a00-8105-11ea-966d-d0b4b95c0a50.png)

Note that BEFORE, the decompiler did not list any passed parameters for the function, depsite parameters existing in memory when the function is called.

Now the function's parameters are properly represented in the decompiler.
