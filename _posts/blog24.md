# What is a buffer overflow?

![image](https://user-images.githubusercontent.com/20525440/80857427-f6692600-8c06-11ea-97bd-8e6ddebcb540.png)


A program in execution works by shifting values to memory instructions and manipulating those values via various operations such as mov, add, jmp, etc.

The way we keep track of those instructions is via the instruction pointer.

## What is the actual exploit though

Now: Say we have a program that calls another function. Such as reading input from the user or trying to print a string.

Any time that has to happen, the program has to know where it originally came from, so it uses something called "the return address".

This is important.

If you can find a way to change the return address, we change which "instruction" the program is reading from. Thus, you change the program's execution

Thus we have: The buffer.


When a programm reads in data, it has a certain amount of memory allocated to it: The buffer.

If you put more data than is intended, you can go PAST the buffer and attempt to fill the return address with your own.

That way, you can try to return into data that you used to overflow the buffer in the first place, leading to code execution by putting in values that would start malicious processes

![image](https://user-images.githubusercontent.com/20525440/80857767-4c3ecd80-8c09-11ea-9aed-73f94965c135.png)
