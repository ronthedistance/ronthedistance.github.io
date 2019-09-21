# Blog 2 [9.20.2019]
### Week 3

Unfortunately, due to procrastination I don't seem to have as much going on for this. 
I have however started some work on the ansible playbook and would like to break down the little I have.

### Attempting a playbook.
![ansible-logo](https://user-images.githubusercontent.com/20525440/65368429-c9c34980-dbf5-11e9-8fb4-987d80dc7c80.jpg)

![playbook](https://user-images.githubusercontent.com/20525440/65368440-dcd61980-dbf5-11e9-8c92-7229f3dd0603.png)

The first element you see here is ```hosts```
Hosts specifies host groups, and defines WHERE the "plays" or "tasks" are going to happen. 
In our case, every task is going to be performed on our local host machine.


The second element you see here is called ```tasks```
This basically sets up a code block, similar to indent-based code blocks in python.
Tasks are basically like commands that are about to be run by a playbook, in addition to how they will be performed.


Tasks have to be named by the ```name``` section.
this is preceeded by a single dash, and must fit correctly within the code block defined by the task section.
as a side note, indentation is ***REALLY*** important in yaml, and it's really screwing with me.

The 4th section is supposed to be pointing to the ```become``` section, but I'm currently not going to change this, as I also procrastinated in making this blog post, and wish to complete it before it's submission date.
Become is used within a task to specify a user. I believe in the case of this current playbook, the set user is "super"

The 5th section that was meant to be pointed out is ```copy```
Copy is automagically known by the "tasks" section. The action is mostly self explanatory.
In the case of our lab, it is similar to the symlink workaround from the previous lab.
By copying the information from the ....../US/Pacific file, we're able to eventually get around the issue with interaction during the php install.
Is it necessary? No, because we could just make the php install non-interactive.
But it's just useful to have NTP set up beforehand anyway, so we're just going to keep this.


The last section is the ```apt``` section of this task.
What's special here is that instead of writing out individual items, we can pass in a list.
within the brackets we have the ``` {{ item }}``` parameter.
Under it, we can call a list called with_items to pass in packages to download.
with state defined as latest, we can get an organized list of updated packages to download.
Plus I think the way ansible has it listed looks really nice.


### Other news
Turns out the first team I'm starting my rotational internship is going to be the enterprise vulnerability management team.
If you thought you knew how much Disney owns outside of media, you're in for a shock.
Unfortunately I'm not sure what is public domain information, so I'm gonna let  that one sit.

Additionally, it's always good to remember that security is a business function.
As much as we want to be technically secure, everything comes down to the fact that security is inverse to convenience.
Security should not impact revenue more than the actual risk involved.


### other other news

I really want a pet bird;probably a conjure. This is random but i'm wondering if anyone actually reads this to the end to find out.