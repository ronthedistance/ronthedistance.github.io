# Blog 13 [1.31.2020]
A shorter post than the previous blogs -- I am attempting to get back into the mix of things.
## "Clones of the outer repository will not contain the contents of the embedded repository and will not know how to obtain it."

![image](https://user-images.githubusercontent.com/20525440/73587742-4b649700-4474-11ea-939a-3920d7ac3150.png)

A small issue came up during work that I was embarased at taking over an hour to troubleshoot.
It was hard to google 'gitlab icon turned into bin looking icon after changes to .gitignore'

## The scenario

After makes changes to .gitignore to ignore some json files and subdirectories in certain folders, the icon changed from a folder to what I think looks like a storage bin.
Doing so makes a user unable to click on the directory name to see the files inside.
When pulling or cloning the repository, the folder is viewable from the web-gui but cannot be referenced from the command line, and its contents will be empty.

## How did I end up troubleshooting ?

I made a random change to another file, ran git add ./ and then came to this new error that cleared things up a bit.

![image](https://user-images.githubusercontent.com/20525440/73587983-a8ae1780-4477-11ea-8ec1-96a33ff202ea.png)

It seems that github thinks there's a subdirectory inside the folder that I mentioned before.

As this was a work laptop, I can't take a screenshot, but as soon as you change directories into the problem folder, the branch name will change.
In my case, it changed to "ae691512", a branch I never remembered creating.

## Actual issue

At some point, someone was in this directory for whatever reason and ran git init.

After doing an ls -l I found there was a .git dotfile that was completely empty in that subfolder, but messed with git because it then thought that folder was a completely separate repository.

From the webGUI, the icon changed to the storage bin icon, which I now know represents a submodule.

## Solution

- rm -rf the empty .git directory
- cd to the root directory of your repositry
- rm -rf --cached . 
- git add .
- git commit -m "Add a message of your choice"
- git push
