You may run into issues when pushing code to a repository. You may need to provide credentials to Git.

This process is fairly straightforward but can be slightly confusing, so let's go through it.

git is a command line tool. If you need to push code into a GitHub repo, you need to prove that you have write access. How do you prove that? By providing credentials (i.e. your github login info). 

However, GitHub disabled using passwords for access from the command line a while ago.

All you have to do is log into your account, go to Developer Settings, Tokens, and generate a GitHub API key. Give it the permissions you need, set the expiration to whatever you want, generate the key. Then, when the git command line tool asks you for a password, just paste in that key instead. Git for Windows should save this in the credential manager and not ask you again. GitHub Desktop, I think, just lets you use your password.

When all is said and done, you should be able to freely push to your own repos.

