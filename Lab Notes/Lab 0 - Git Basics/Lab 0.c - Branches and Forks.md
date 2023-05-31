Git follows a tree-like structure. This is nice, because it lets you have multiple people working on different branches independently and makes the merging of those branches generally fairly easy.

Note that, on GitHub, a repo has its own branches. A *fork* is basically a copy of that repo (belonging to you instead of the original repo owner), which looks to the original upstream repo for updates. **The git tool itself has no knowledge of forks. "git clone" sees the original repo and the forked repo as two completely unrelated things.** 

A *pull request* is a request for someone to pull your changes into their repo. It is common when working on open source projects to first ask if you can contribute, then to open a pull request with your contributions. This will provide a list of changes and a description you provide, and the owner of the target repo can choose to accept the changes, ask for fixes, or to reject your pull request.

Because forks are a separate repo, and each repo has its own branches, things can get confusing fast. Sometimes it is better to localize your concerns to just what you are working on.

For this reason, a common way of working is to do the following:

1. You and your team members each fork the "upstream" repo. In small teams without a central organization (like students in this class), one person makes the repo first, then others fork their repo. The main/master branch on this repo is considered the central location.
2. At this point, there may be an original repo, say Getkablue/OurCoolProject, and two forked repos, JayceTheBased/OurCoolProject and fishmanssu/OurCoolProject.
3. Each team member runs "git clone" on their computer pointing at *their forked repo*.
4. (optional) Each team member (besides the original repo owner) adds the original repo as a *new* remote called "upstream", like:
   ```
   git remote add upstream https://github.com/Getkablue/OurCoolProject.git
   ```
5. Each team member now works on their own local copy. When they push changes, by default they go to the remote called "origin", which by default points to their fork. These changes are then available on GitHub.com.
6. From there, the person making the contribution opens a *pull request* between their fork and the original repo. This will create a notification on the original repo listing all the changes and a description provided by the team member. The repo owner can approve and merge the PR, can ask for changes, or can close it.
7. Once accepted, the changes are now in the original repo. But now some team members may not have the changes present in their fork or in the local machine. In order for everyone to sync up and pull those changes, the other team members need to click *sync changes* on their repo on GitHub to pull the updates. Then, they can run "git fetch" and "git pull"  on their computer to pull the changes.

This workflow may seem slightly overcomplicated. The truth is that yes, it does add overhead, but it provides a very important advantage, especially for small teams and beginners: Everyone has their own "saved" copy on GitHub, and their own "working" copy on their machine. This makes it much more difficult for anything destructive to happen to the repo.
