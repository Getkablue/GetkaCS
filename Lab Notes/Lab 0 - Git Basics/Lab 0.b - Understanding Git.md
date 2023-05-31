We will use Git to share code and work collaboratively in this course. While it is not critical to CS in general, knowing the basics will help you back up your code, keep logs of your progress, and allow you to work efficiently with others.

Git is a tool which allows you to set up a project (or *repository*) with some code in it, make changes to it which are individually tracked (*commits*), and push your project to a central location (sometimes called *master*). When using git, your changes will be tracked in a tree-like structure which keeps a record of every commit ever made.

Why not just use any plain backup service, like Dropbox? Let's consider a hypothetical.

Let's say I am working on a document with my friend, Nigel, and just spent 5 hours editing it. I go to upload the file and successfully upload it. Yay! But wait -- Nigel was working on the document, too. He opened it up 6 hours ago and added a smiley face, then forgot about it because he had to go do a welding job. When he gets back to his PC, he saves the document. Oops: My 5 hours of progress are now gone. And sure, Dropbox might have backups. But that isn't a sure thing. Even if there are backups, we have to go manually work together to integrate our changes into the same document. Blech.

Git allows you to jump between tasks, or even have multiple people work on different tasks simultaneously. And changes that you make are never reflected on the master until you explicitly choose to push them there. This ensures that only the changes you want to share get shared, and makes you look like a lot less of a fuck-up.

Here's the way this all works, in the simplest way:

You create a central location for your code on a site that supports git, such as github.com. When you first create that repository, it's empty. This is the *remote repo*. 
You *clone* this repo to your local machine. This creates a directory on your PC which would contain all the code (the *local repo*) -- however, at this point, it's blank. The master repo *has no knowledge* of your local repo, but your local repo knows where the *remote* is and how to talk to it.
You make changes to the code in the local repo, then *add* the files to have Git track them. You *commit* these changes, saving them to the local in a tracked way. Any files which you changed but didn't *add* will remain untracked. 
Then, when you are happy with your commits as a whole, you *push* them to the remote. Assuming everything goes well, your code is now visible on github.com.
Now let's say your friend, Jason, wants to contribute. He goes through the same process, except he starts with your repo instead of making a blank one.
After he pushes his commits, you can *pull* them to your local, so you can try them out and add more to them. 
The cycle continues.

