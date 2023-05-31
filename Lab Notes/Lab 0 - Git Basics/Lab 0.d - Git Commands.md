There are plenty of good resources on git commands online. Rest assured that no git command will ever touch files that are not in a git repo, so you are free to experiment. Just don't go running around running "git init" on your C drive's root directory. Be reasonable, keep git repos in a specific location and you will be totally fine -- the worst case scenario is that you lose work on these assignments. Really not a big deal. 

I strongly recommend that you use the online documentation as this note sheet cannot possibly be exhaustive. Don't worry about all the available commands -- that will just overcomplicate things. Just worry about what you want to do and google it. I guarantee someone has asked how to do the same before. You **will** eventually become comfortable and the basics will become second-nature, and you'll be ready to understand all the power-user stuff *when you need it*.

If you are really that uncomfortable, use a Git GUI software like GitHub Desktop.

Of course, be sure to review the basics on working with a terminal:

To change the current working directory:
```
cd C:/Users/Alex/Documents/

cd ./MyVeryImportantStuff
```
(This will first bring us to C:/Users/Alex/Documents then to C:/Users/Alex/Documents/MyVeryImportantStuff)

To list the contents of the current directory:
```
ls -l
```
(This works powershell or bash -- try just "dir" if using windows cmd)

The most useful commands for day-to-day working with git:

To start a new, blank git repo in the current directory:
```
git init
```

To clone a repo, say from GitHub:
```
git clone https://github.com/Getkablue/SampleRepo.git
```
(This will make a new directory called SampleRepo in the current working directory)

To add new files to your repo so they are tracked:
```
git add ./mynewfile.py
```
You don't need to do this unless you made a new file which is not yet tracked.

To remove a file from tracking (this doesn't necessarily delete it, just removes it from the repo):
```
git rm ./myoldfile.py
```

To pull updates from the remote:
```
git pull
```
(You may first need to pull metadata changes like new branches with "git fetch")

To commit your changes:
```
git commit 
```
OR
```
git commit -m "my commit message"
```
The former will open up a text editor for you to write a message, the latter sets it explicitly.

To push your committed changes to the remote:
```
git push
```

To list current branches
```
git branch
```

To create a new branch from the current one
```
git branch -b my-new-branch
```

To swap to a different local branch:
```
git checkout my-other-branch
```

To check out a branch from the remote (like for testing a feature someone else made)
```
git checkout origin/that-cool-branch
```

To check the current status (any files added but untracked? Which files have been changed but not committed?):
```
git status
```




