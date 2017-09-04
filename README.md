# Yet another git introduction

## What is git?

git lets you keep track of different versions of files in a git **repository**. You can also optionally publish your files to an online service like github.

## Checking if git is installed

Type the following command in your terminal.

```
git version
```

You should see something like:

```
git version 2.11.0
```

The exact version doesn't matter. If you get instead get an error, you should install git before continuing. The command to install git on Ubuntu (or other Debian versions) is

```
sudo apt-get install git
```

## Making a git repository

A git repository is just an ordinary folder on your computer that has some special structure added to it.

To create a repository, first make an ordinary folder. Type the following commands in your terminal.
```
cd ~
mkdir myfirstgit
cd myfirstgit
```
These three commands change directory (abbreviated `cd`) to your home folder (abbreviated as `~`), create a `myfirstgit` folder, and then change directory to `myfirstgit`.

To turn your `myfirstgit` folder into a git repository, type the following command.
```
git init
```
You should see something like:
```
Initialized empty Git repository in /home/user/myfirstgit/.git/
```
Congratulations! You have made your first git repository.

If you want to remove your first git repository, or run into trouble and want to start over, you can delete the repository you just made by typing the commands:
```
cd ~
rm -rf myfirstgit
```
After which, you can return to the beginning of this section to start over and make the git repository again.

## Basic git workflow

The most basic way to use git consists of the following steps:

1. Use `git add` to add versions of files you are happy with to the git repository.
1. Use `git commit` to mark a point in time you may wish to return to.
1. (Optional) Publish your new file versions using `git push`.
1. (Optional) If you're working on a team, retrieve new versions made by others using `git pull`.

### Adding files to the git repository

git will only keep track of files you tell it to. First, create a file in your `myfirstgit` folder:
```
echo Hello > test.txt
```
This command will write the word "Hello" to the file `test.txt`. Now type the command
```
git status
```
You should see
```
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	test.txt

nothing added to commit but untracked files present (use "git add" to track)
```
We can see that git has noticed a new **untracked file** in your repository and is telling you to use `git add` to start **tracking** this file. When git is **tracking** a file, it will keep track of your different versions of this file. Let's tell git to start tracking your new file with this command:
```
git add test.txt
```
Like most commands, if nothing has gone wrong, you won't see any output. Let's try `git status` again. You should see:
```
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   test.txt
```
Now we can see that git is ready to **commit** one new change: adding your new file. Congratulations! You have now added your first file to your git repository. But your file version isn't saved yet: to do that, you need to make a **commit**.

### Making a commit

git measures time by **commit**s. Each **commit** marks versions of your files you may wish to recall later. You make a commit after you've used `git add` to add all the files you're happy with. Later, you can recall the versions you `add`ed by using `git checkout` (more on `git checkout` later).

Make a commit by typing the command:
```
git commit -m "added my first file to git"
```
If this is your very first time using git, you will see an error:
```
*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: empty ident name (for <(null)>) not allowed
```
This error means git doesn't know your name and email address, so it can't fill out the commit's author fields. To tell git who you are, type these two commands with your information filled in.

**Be careful!** If you publish to github, the email and name you provide here will be published along with your commits. Use information you don't mind being made public.
```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```
Now type your commit command again, or use the up arrow on your keyboard to find it in your command history.

```
git commit -m "added my first file to git"
```
Each commit must have a message attached to it, that's the part after `-m` and in quotes. This is to let others know what important changes have been made in your files. If you want to write longer commit messages, you can just type `git commit` and git will open a text editor for you to type your message. When you're done, save and close your editor, and git will make your commit.

Let's try `git status` again. You should see
```
On branch master
nothing to commit, working tree clean
```
Now git is telling us that we have no new changes to commit (because we already committed our new file) and that the **working tree** is **clean**. The **working tree** is your `myfirstgit` folder, and it's **clean** if git doesn't detect any changes to your files. 

### Making some changes

Now let's change our test file.
```
echo Bonjour > test.txt
```
This will change "Hello" to "Bonjour" in test.txt.

Let's try `git status` again. You should see
```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
Now we can see that git noticed a change in our file. We can ask git what's changed by typing the command
```
git diff 
```
This will show the changes made to to all the tracked files. You should see
```
diff --git a/test.txt b/test.txt
index e965047..632e4fe 100644
--- a/test.txt
+++ b/test.txt
@@ -1 +1 @@
-Hello
+Bonjour
```
The `-Hello` is telling use that "Hello" was deleted, and `+Bonjour` is telling us "Bonjour" was added. When you are done reviewing the changes, press `q` to return to your command line.

Since we are happy with our changes, let's `add` and `commit` them.
```
git add -A
git commit -m "added french version"
```
This time, we are using `git add -A` to tell git to add all changes in our repository. This can be useful if you change multiple files and want to add them all at once.

**Be careful!** `git add -A` will add every file in your repository, including any temporary or private files you may not wish to publish. If you only want to commit specific files, use `git add filename1 filename2 ...` to tell git exactly which files to add.

If you try `git status` again, you should see:
```
On branch master
nothing to commit, working tree clean
```
Now git is saying there are no new changes. Our working tree is clean again, since we just committed all our previous changes.

### (Optional) Publishing your work
If your git repository is set up to publish your work to github, it's easy to publish (called `push` in git) your commits.

First, create a new github repository through the github website. Then, github should show you some commands to "push an existing repository from the command line" that look like
```
git remote add origin git@github.com:YourGithubAccountName/RepositoryName.git
git push -u origin master
```
Of course, your account and repository names will be different. If you type these commands, you will be prompted for your github password, and then your changes will be published.

Later, if you make a commit you wish to publish, you just need to type the command
```
git push -u origin master
```
and your changes will be published.

### (Optional) Working on a team
If other people are publishing commits to your github repository, you can download their commits by typing
```
git pull
```
<!---
TODO dealing with conflicts and merges...
-->

### Workflow summary
To summarize the above steps, your basic git workflow is
```
git pull # Optional
(...work until you are happy...)
git diff
(...review your changes, press q when done...)
git add -A
git commit -m "describe your changes here"
git push -u origin master # Optional
(...continue working until you are happy again, and repeat...)
```
If you don't have (or don't want) commits from other team members, leave out the `git pull` command. If you don't want to publish your work, leave out the `git push` command.

## Reviewing repository history and recalling old versions of files
The advantage of using git is that you can review your commit history, and return to any previous commit you made.

Before starting, make sure your working tree is clean by typing `git status`. It's always best to start with a clean working tree before recalling older commits, as git will refuse to overwrite any uncommitted changes you have made. If your working tree is not clean, use `git add` and `git commit` to commit your changes before continuing.

To see a log of all your past commits, type the command
```
git log
```
You should see
```
commit 0a0bebf35cbbb23ad281dae62747d4eaf714d52f
Author: Jonathan Schroeder <jd.schroeder@gmail.com>
Date:   Mon Sep 4 10:49:50 2017 -0400

    french version

commit a0aa9366c4478a7abdf6dfe7fc6780d9325aee63
Author: Jonathan Schroeder <jd.schroeder@gmail.com>
Date:   Mon Sep 4 10:35:47 2017 -0400

    added my first file to git
```
Here git is showing the two commits you made while following this guide, along with the time they were made and the commit messages you provided.

When you are done reviewing the log, press `q` to return to the command line.

The long string of letters and numbers after `commit` is the **commit-id**. We can use the **commit-id** to return the repository to a previous commit. The commit-id of my first commit is `a0aa9366c4478a7abdf6dfe7fc6780d9325aee63`. 

**Be careful!** Your commit-id may be different than mine, so substitute yours into the following commands. 

To return to the first commit, use `git checkout [commit-id]`
```
git checkout a0aa9366c4478a7abdf6dfe7fc6780d9325aee63
```
You should see
```
Note: checking out 'a0aa9366c4478a7abdf6dfe7fc6780d9325aee63'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at a0aa936... added my first file to git
```
Now display the contents of test.txt by typing the command
```
cat test.txt
```
You should see
```
Hello
```
instead of "Bonjour," meaning git has restored the repository to its state when you made the first commit.

To return to the most recent version of the repository, type
```
git checkout master
```
You should see
```
Previous HEAD position was a0aa936... added my first file to git
Switched to branch 'master'
```
Again, display the contents of test.txt
```
cat test.txt
```
This time, you should see
```
Bonjour
```
meaning git has restored the most recent version of `test.txt`.

### checkout summary
Summarizing how to use `git checkout`:
```
git status
(...make sure working tree is clean, use git add and commit if needed...)
git log
(...choose which commit you want to return to, press q...)
git checkout [commit-id]
(...look at the older version of your repository...)
git checkout master
(...back to the latest version...)
```
