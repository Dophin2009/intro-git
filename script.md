## Title Screen
Before I start...
1. Who here has at least heard of Git?
2. Who here has used Git before?
3. Who here uses Git on a regular basis?

This is probably one of the most important tools you might ever learn related to software development,
so you'll probably want to pay attention. 
It's used on a daily basis by professional developers, and even if you don't plan on going to into 
software development, it's extremely useful in any project.

## Version Control
Before we talk about Git specifically, I'll talk about version control in general, 
which is what Git is a type of. 
Version control, or source control is software that helps you track and manage changes.
At its very core, it allows you to maintain different versions of your code.

For example, let's say your code is working. 
Then, you make a number of changes that screw up your code, and it's suddenly broken.

You might be inclined to `Ctrl-Z` to oblivion until you've gotten back to the working version, 
but more often than not that's not possible.
Maybe saving backup copies is the solution.

Version control solves and simplifies all this: 
- It tracks creation and deletion of files and any changes made to the codebase, 
  and allows you to revert to older versions. 
- It streamlines development by tracing the work of different contributors, 
  facilitating code reviews, and stabilizing development through its various features

### Branching
Besides versioning, one of the most important features of version control systems is branching.
Development is organized into branches, which may serve different uses.

Usually, a project has a main branch, called the **master** branch, 
which is generally held pretty stable.
You may think of it as the trunk of a tree.

In branches created off the master, you can make commits to work on features or serve some other purpose.
Versions, commits, are placed on a separate line, so your code and version history of the mater branch
is not affected.
This means that you can experiment without fear of hurting existing functionality. 

You can create a new branch specifically for developing a new feature of your application, 
while you or others can also continue work on different aspects of the application 
on different branches.

Basically, contributors can work on features independent of other features.
And when these are ready, they can be merged into a single branch.

As you can see in the figure, the developers have separate branches for releases and development,
with branches constructed to work on various features or patches. 
The bottom most branch was being used to develop some feature, but was eventually abandoned.
The other feature branch was eventually merged back into the main `develop` branch.

### Some Version Control Systems
We're going to talk about Git, but first, here are some other examples of source control managers.

One of the oldest and most well-known version control systems is **Concurrent Versions System**.
CVS uses a **client-server model**, meaning that a central server has a master copy of the repository.
Contributors can checkout copies of this central server and then push changes back.
CVS is one of the most well-understood systems, so troubleshooting should be easy, 
but its last release was in 2008.

**Subversion**, also a client-server type, has pretty much all the features that CVS has, 
but also has *atomic transaction* and other improvements. 

Transactions being atomic means that they must be completed. 
If they're interrupted, all changes must be rolled back to the state 
before the initiation of the transaction.

The last of the three, Mercurial, has a **distributed model** instead of the client-server model.
In a distributed model, every contributor's computer has a local copy of the repository.
As such, compared to CVS, transactions are typically faster and cheaper because they are done locally.

## Git
Like Mercurial, Git also has a distributed model; as such transactions are fast and cheap as well.
It also features atomic transactions, file locking, tagging, and others.

Currently, it's the most popular version control software and has been for a while.

### File Statuses
Git's workflow is designed like this:

Files have can four different statuses.

The first of these is *untracked*, meaning it is not under Git's source control.
Changes in these files are not tracked. 
These typically include IDE specific files.

Everything below listed untracked means that the file is under source control.
Git's workflow is split into three sections: 
1. the working directory, or local files
2. the staging area
3. the repository

All active work, the actual coding, is done in the working directory. 
Changed files are added to the staging area.
These staged changes are then *committed* to the repository. 
These commits represent the state of your project at that specific moment.

The benefit of this two-step commit process is that it is easy to select 
and verify the changes you actually want to commit at the moment.
A good way to think of it is as a loading dock right before the cargo is actually packed onto the ship.

You can also save work at stages when you might not want to commit immediately. 
Perhaps you make other changes on the same files after staging, 
but later want to revert back to the better state it was when staged.

Once you've determined what you'd like to commit, you can actually perform the commit. 
This saves your changes into the repository, giving this particular instance 
of your project a hash and other metadata.
In the future, you can view the changes made in this commit for reference, 
or even revert back to this state if things go awry. 

## Itinerary
In this presentation, I'll discuss these points:
1. A step-by-step walkthrough of installation for Windows; Mac should be relatively simple
2. Initialization of a git repository
3. The various commands needed to use Git's functions
   
I was originally also going to discuss remote repositories and I even made slides about them,
but in the interest of time and due to the fact that I ran out of time to write notes on it,
I'm only going to discuss local repository functions.

## Installation
Windows doesn't come shipped with Git, so we'll first have to install it.

Those with their own laptops should follow along actively from the point onward.
It's a good idea to get experience with the commands yourself. 
Those with school laptops should find someone with their own to look off of.
Please, if you have any questions, **ask**.

To keep it simple, we're just going to use the regular installer.
There's also the option of the portable distribution for having a custom install location.

Go to the following link ([https://git-scm.com/downloads](https://git-scm.com/downloads))
and download the latest release (2.21.0) for Windows.
This will give you the download for the 32-bit version. 
If you would rather have the 64-bit version, cancel the download 
and instead click `64-bit Git for Windows Setup`.
I don't really know the difference, but I'd assume the 64-bit version 
can process data more efficiently.

Keep pretty much all the defaults as you go through the install wizard. 
However, make sure that the default editor is set to the **Nano editor** 
unless you are familiar with any of the other choices.
I can't guarantee effective troubleshooting help if you select a different option, especially vim.

### Verifying Installation
To verify that you've installed it correctly, open a command prompt anywhere 
and enter: `git --version`.

If all went well, `git version 2.21.0.windows.1` should be printed.

If not, then you probably need to edit your environment variables.
1. In the start menu, type `environment variables` and select `Edit the system environment variables`. 
2. At the very bottom, click the button `Environment Variables...`.
3. In the lower section, titled `System variables`, scroll until you find the variable named `Path`.
4. Double click, or click to highlight and click `Edit`, 
   which should bring up a menu containing all your Path variables.
   These are usually binaries that your system can access.
5. Click `New`, then `Browse`. Navigate to the install location of Git, 
   which is mostly likely `C:\Program Files\Git` and then select the folder named `cmd`. Click `OK`.
For 32-bit installations, use `Program Files (x86)` instead of `Program Files`.

Exit all these windows and then try getting the version of Git again in the command line.

You can also try installing again just by running the downloaded executable again. 
It should automatically remove your previous installation along the way.

## Usage
At this point I'll go through the most important commands you would use on a regular basis in Git.

There are a lot of different graphical interfaces that make using Git very simple,
but we'll be using the command line so that you can get somewhat of an understanding
of how it works. 
This way, you'll be introduced to the key terms that are used across all Git interfaces,
whether textually or graphically based.

### Initializing
For any project, you need to first of course initialize a repository.

To create your own git repository in an existing project folder or for a completely new project, 
use the command `git init`. 

Let's create a new folder titled `git-test`, open a command line here, and then run `git init`.
This creates a folder named `.git` in your project root, which stores all related data. 

I'd also recommend learning sometime the most common Windows commands, such as `dir` and `cd`.
Learning Unix commands is also a good idea.

Let's create a text file in this new project.
You can do this through Notepad or something, or just type in the command line: 
`echo this is a new file >> file.txt`.

### Git Status
Now that we've created a file, let's verify that this file is untracked.
You can view the status of changes with the command `git status`.

If we run `git status` in our working directory, it tells us that 
`file.txt` is currently untracked by Git.

It also tells you which files are tracked but unstaged, and those that are staged.
Tracked but unchanged files are not listened.

### Adding/Staging
Let's suppose we want `file.txt` to be managed by source control.
The `git add` command stages the specified file or directory, 
effectively placing it under source control.

So, if we enter `git add file.txt`, and then use `git status`, 
we can see that it lies under `Changes to be committed`.

This command is also used when you wish to stage files that a tracked, but unstaged.

To stage all files, subdirectories and their files in your current directory, use `git add .`. 

This command respects the specifications of `.gitignore`, which is an optional configuration file.
In this file, you can specify specific files or patterns to be ignored by Git, as I mentioned before. 
When you try to add a filename included in `.gitignore`, you'll be warned and asked to use `-f`, 
the force modifier, if you really want to add it.

Since this is Windows, and we went with the default settings in the install wizard, 
you might see the warning `LF will be replaced by CRLF in file.txt`.
It just has to do with how Windows and Unix do line breaks differently.

### Committing
Now that our file is staged, and we think it's ready to be committed, 
we can do so using the command `git commit`. 
This, remember, saves the state of our project with the staged changes. 

A commit always needs a message.
Depending on the substance and size of the commit, your message might just be a simple one-liner, 
or it could also have a description.

To simply have a one-line commit message, add `-m "<message>"` to the end of `git commit`.

If you think that your commit needs more clarification than just a single line, 
leaving the command as just `git commit` will open the editor—Nano in our installation—to 
edit the commit message.

Not every commit needs one of these paragraph explanations, 
and it's better to keep your commits small, in accordance to the idea of an "atomic" commit. 
This means your changes revolve around a single task.

For the first commit, people usually write "Initial commit" as the message.

For more info on to write good commit messages, I highly recommend you check out the link there: 
[https://chris.beams.io/posts/git-commit/](https://chris.beams.io/posts/git-commit/).

### Log
Now that we've made our first commit, we can view it. When we've made more commits, 
we can view information about them.
To do this, use the command `git log`.

`git log` brings up a list of all your commits on the current branch, 
as shown on the upper right.
We can see the author of the commit and when it was done.
Information about branches can also be seen: `HEAD -> master` means that that commit is the *head*, 
or most recent one, of the master branch.

Use the modifier `--oneliner` to show only part of the commit hash and the summary message, 
as shown in the lower right.

### Reset
Let's change our `file.txt` a little bit by doing: `echo made some changes >> file.txt`. 
This adds a new line below our original that says "made some changes".

Suppose we think our changes are good to commit now. 
We do: `git add file.txt` and then `git commit -m "Make a change"`.

Now we have two commits. But now realize that our change was bad: 
it's reduced application efficiency or stability or something.
To reset everything back to the previous commit, we use `git reset`.

If we do `git log` to find the hash of our first commit, then do `git reset --hard <commit>`, 
we are moved and the working directory is modified to match that of that commit.

There are subtle differences between the `checkout`, `reset`, and `revert` commands that 
make them all perform slightly different actions, and it's a good idea to look into that a little. 

### Revert
`revert` is arguably the safest way to undo changes.
It creates a new commit that patches out the changes made in the specified commits.


There are different ways to write it:
- `git revert <commit_1> <commit_2> <commit_3> ...`
- `git revert <range_start>..<range_end>`

In contrast to `reset`, it doesn't rewrite commit history.

If you want to just completely start over, you might want to use `reset`.
Otherwise, use `revert`.

### Checkout
`checkout` is used mostly to switch branches.
It can also be used go back to a specific commit, where a new branch can be 
created or files can be viewed.

If you have modified, but unstaged files, Git will ask you to either stash or commit
these files before checking out.

When you checkout to a past commit, you enter a detached head state,
meaning that if you make new commits at that point, your newer commits will be lost.

### Branching
Remember, branching is useful for organizing development of different features into 
separate zones of experimentation.

To create a new branch at your current commit, use `git checkout -b <branch>`.

To switch back to a different branch, use `git checkout <branch>`.

On these branches you can work on different features, and then, when you think it's 
ready to be added back into the main branch, you use `git merge`.

`git merge <branch>` takes the changes made in the target branch and copies them 
into the current branch.
If you want to merge some new completed feature back into the master branch,
you would checkout to the master branch and then call the command there.

So, let's make a new branch to demo on called "new_feature" with `git checkout -b new_feature`.
Add another line to `file.txt` with `echo code for new feature >> file.txt`.

Still on the `new_feature` branch, we'll `git add file.txt` and then `git commit -m "Start new feature"`.
This commit is now on the `new feature` branch, but does not exist on the `master` branch.

If we`checkout` back to master, the addition to the file disappears.
Those changes don't exist on this branch.

Let's suppose we think that this feature is ready to be added back to the main branch.
We first do `git checkout master` to switch to the accepting branch, which would be `master`,
and then call `git merge new_feature`.
Its changes are added to `master`, and then at this point, we can delete our `new_feature` branch:
`git branch -d new_feature`.

## Closing
So that's it for now.
The subsequent slides are related to remote repositories.
You can either look at these slides yourself, or if there's interest, 
I can make a second talk on them.

At the very end are some extra things that I made slides for, 
but didn't deem worth squeezing into the actual presentation.

If you're overwhelmed by all this information, all the different commands
and their different uses, don't worry: that's expected.
A lot of this can be really difficult to wrap your head around.
I don't understand how a lot of things work in Git either.

If the command line stuff is too daunting to use, most modern IDEs have solid Git integration.
For example, Visual Studio Code has built-in source control.
There are also tons of extensions that improve the functionality.
Through the IDE's convenient GUI, you can stage, commit, and do all the other things
you would do in the command line.
There's also GitHub Desktop, which is very common.