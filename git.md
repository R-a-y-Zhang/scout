# Scratching the Surface of Git
## Description
This will give a very basic overview of what is Git and how to use it. This will cover common
tasks when using git. However git is extremely complex so feel free to delve deeper.

## git - the stupid content tracker
Git is a VCS, or **V**ersion **C**ontrol **S**ystem, and is widely used by nearly every single
organization to maintain and keep track of their codebases. It was developed in Apr. 2005 by
Linus Torvalds after the developers of the Linux Kernel moved away from BitKeeper since the
service revoked users' ability to use the service for free. And thank god for that because if
not we would not have git.

As a VCS, git is able to keep track of every single change made to a repo, who made it, and
what lines were changed when they made it. In addition, it allows people to work on separate
parts of the project completely independent of one another, allowing for parallel development.
Last but not least, it enables extremely fast compression and decompression of codebases through
delta compression and multithreading.

## Basic Git Codebase Structure
A git project is called a **repository**, and one can think of a repository as a tree. Like an actual
tree, like _Quercus rubra_,  not a data structures tree. A repository is split into branches and
one of those branches, called the **trunk**, contains code, or at least should contain code, that
is officially recognized to be part of the codebase. This code should be proven to be relatively bug
free, at least upon first glance, should be passing all unit tests, and should be seen by the maintainers
as acceptable. This is usually called the _master_ branch, _main_ branch, or some other name synonymous
with those two words (I will be, for the sake of simplicity, be using _master_ to refer to the main branch).
In addition to the trunk are other branches called development branches, which contains
code that people are currently working on or things that were worked on but have been shelved temporarily.
These are simply called _branches_.

## Pulling and Cloning
When you wish to retrieve the codebase from a git hosting service, whether GitHub or otherwise, you need
to **clone** the repository. You will be provided with the proper url and will do a ```git clone <url>```.
This will copy the repository from the host onto your local machine, making a folder that is the name
of the repository. Going into that folder you will see a _.git_ folder. This is what git uses to do
whatever it needs to do and almost always you **DO NOT** touch this folder. A lot of the times, you should also see
a _.gitignore_ file. This file tells git what ignore when it is tracking the current repo.

## Staging, Committing, and Pushing
Let us say you have done work on the repo and you are now ready to update the repository on the host
for others to use or so your team can be updated on your progress. You will have to now do three things:
Stage, Commit, and Push.

**Staging** code is putting code that you want to be in your next commit into the staging area. This can be
files, folders, etc. The process to do this is to execute ```git stage <files|folders>```. If you need information
about what files you have changed, you can execute ```git status``` and it will list files you have modified
and files that are newly created. Doing a status check before doing a git add is very
useful because you can check to see if there are any files in there you that
you do not want git to be tracking. Common things that git should not be tracking
include configuration files that have passwords and sensitive information,
.db or database files, log files, bytecode and compiled files (.o, .class, .pyc, etc.),
to name a few. If you see these, then you should add these into your .gitignore,
run a ```git status``` to make sure that they actually are not being tracked, and then
doing ```git add```. It is highly advised that you do not stage all your changes at once
but to stage the ones that are relevant together, commit them, and then stage the next
group of files that are relevant.

**Committing** your code is the next step. This is an _extremely_ important step as
this is where the _version control_ comes in. By committing, you are creating a historical
record of changes made to the repo and people can see what changes were made and what those
changes were by running ```git log```. This also allows you to go back to a previous commit
if after a few new commits you find yourself not happy with them. To commit what you have already
staged, you would run ```git commit -m <short-commit-message> -m <long-commit-message>```.
The short commit message is a very short description of what you did like "Fix feature X",
"Remove X", etc. The long commit message you can go into a more in-depth overview into
what you did. Proper etiquette is to include both. **COMMIT OFTEN!** Also, standard
practice is to use present, imperative tense in your commit messages.

After you commit, it is time to **push** your code to the remove host repo. To do this,
just do ```git push origin <branch-to-push-to>``` and git will automagically sort out
what the best way to do update is. Note, the branch-to-push-to will almost always be
your current branch. To check what branch you are on, execute ```git branch```. However,
before you push, you must pull.

**Pulling** is where you pull code from the remote machine to your local machine. You
MUST do this before every push unless you know you are the only one who is working on the
branch (even then, it does not hurt). This is very similar to clone but there are some
minor differences that you can google. To pull, just do ```git pull origin <branch-to-pull-from>```
where the branch-to-pull-from will usually be the branch you are currently on.

## Branching
In order to allow for various aspects of the repo to be developed separate from one another
and also being able to keep track of all those changes, git allows for one to **branch**.
This is key to any VCS. When you branch, you essentially peel off from you current branch
and any commits you do will be to the new branch that you created instead of the old branch.
Let us say that you are making a feature and you want to branch off the master branch. You
would first go to or **checkout** to the master branch by executing ```git checkout master``` and
then would execute ```git pull origin master``` to get the latest changes to master. If you
want to checkout to a branch that the remote repository has but you do not (you can find this
out by executing ```git branch``` and it will tell you all branches that are recognized by
your local machine and the one you are currently on will be marked), you need to do
a ```git fetch origin <branch>```, then ```git checkout <branch>```, and finally
```git pull origin <branch>``` to get the latest commits. Now, let us say you have
successfully checked out to master and the pull was successful. Now you need to make a new
branch. To do that, you would do ```git branch -b <new-branch>``` which will create
new-branch and checkout to new-branch. Now, whenever you stage and commit you will
be doing it on new-branch.

## Issues when pulling from a remote machine
When you try to pull, git does two things: 1) checks to make sure you do not have
any uncommitted changes and 2) makes sure that there are no conflicts in your code.
If git finds that you have unstaged changes, you can (and you probably should) just
commit them and then try again, or you can do a ```git stash```, which essentially
puts your code in temporary storage and after you pull you can run a ```git pop```
which will pop off the stashed code and apply it to your current repo. Now,
every once in a while, you will encounter what is known as a **conflict**, which
is where you and someone else had edited the same part of the code and git has no way
of figuring out how to properly reconcile the new changes. Then, it is up to you to decide
decide which ones to keep and which ones to throw away. There are a few ways to
do this. One way, is to manually go into the affected files and resolve the conflicts.
To do this, open up the affected files and you will see something along these lines:
```
<<<<<<< HEAD
// some code 1
=======
// some code 2
>>>>>>> <branch>
```
This represents where the conflict has happened. Everything above the equal signs
are the changes currently in the HEAD or the latest commit that you committed,
and everything underneath the equals signs are the latest changes in the most
recent commits in the remote repository on <branch>. To resolve this, simply delete
the code that you do not want as well as the ```<<<<<<< HEAD```, ```>>>>>>> <branch>```,
and ```=======```. Then you readd and recommit the files before trying to git push.
If your git version is 2.12 or later you can just do ```git commit --continue```
instead of readding and recommitting.

The other way, is if you know that you only want your changes or only want
the changes from the remote machine to persist, you can do
```git checkout (--ours|--theirs) (filename)```, using ```--ours``` to keep ours
and ```--theirs``` to keep their changes. The filename can be specified if for
certain ones you want to keep yours and for others you want to keep theirs.

Another way is to use ```git mergetool```, which will present to you GUI regarding
the conflicts, which is much nicer to use than a text editor IMO, and from there
you can do the resolving and whatnot. I have never used this since I just learned
about it while doing this writeup but will certainly try it the next chance I get.

There are probably other methods one can use to resolve merge conflicts so feel
free to google them and figure them out.

## Merging
**Merging** is the process by which one branch is merged or combined with another
branch. You can also do what is called a **rebase** but I have not used that so
I will skip that one for now. When you need to merge, first thing to do is to
push all your changes to the remote repo, then checkout to the branch you want
to merge into, and then do ```git merge <branch-to-merge>``` where
branch-to-merge is the branch which you want to merge into the current branch.
So, if I had just finished work on branch B and wanted to merge into branch A,
after I have pushed everything to B on the remote repo I would do
```
git checkout A
git merge B
```
This might result in merge conflicts which you will then have to resolve.
**WHEN WORKING ON A TEAM, YOU SHOULD NEVER BE MERGING YOUR CODE INTO ANOTHER
BRANCH WITHOUT THE OKAY OF THE BRANCH OWNER OR MAINTAINER. DEFINITELY DO
NOT MERGE INTO THE MASTER BRANCH WITHOUT THE APPROVAL OF PROJECT MAINTAINERS
OR MANAGERS**. To make merges into master, you would submit a pull request
which can be done through GitHub.

## I Made an Oops!
Let us say that you staged some files that you did not mean to stage.
To unstage those files, you can run ```git reset (filepath)``` to unstage your
files or you can specify a specific file to unstage. What if you committed
and something was wrong with it. Maybe the files were not what you wanted to
commit or a part of your commit message involved cursing your boss and their
entire family, then you can do ```git reset HEAD~```. If you already pushed?
Google is your friend.

Now, what about the case where you want to go to a previous commit because
you realize that the changes that you have done so far you do not really like?
Well, to do that you would first run a ```git log``` which will show you
all previous commits, their commit messages, and the commit hashes. When you
find the commit you want to revert to, make note of the commit hash. It is
extremely long but if you pick out the first ten characters that should be
fine. If you want you can use the entire commit hash. Then, to revert to that
commit, execute ```git checkout <commit-hash> .``` to revert to that commit.
The period at the end tells git to apply the revert to the whole repo.

## End Notes
This is all I have for now, if I think of anything I will add them to here.
**If I am wrong on anything, please notify me and I will fix it ASAP.**
## EDN
git gud
