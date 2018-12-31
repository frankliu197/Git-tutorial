
<h1> How to use git </h1>

## Prerequisite knowledge:
* Basic Terminal Commands and Usage (Tutorial Here)
* Understanding of basic programming concepts (such as Variables and Pointers) 
* General project workflow

If you have been programming for some time, you certainly would have heard about git. Git is a version control software that can be used to greatly simplify the process of developing software. Some benefits of using a version control software include:
- Reverting your code to a previous states if something goes wrong
- Multiple developers can work on ONE project without interfering with each other

This tutorial will concisely explain the usage of the common (as well as the not so common) git commands on the Linux terminal. An example project that we will use git to code is a calcualtor, where its changes can be visualized as the following:  
<img src="calculator graph.png" alt="Calculator Commit Graph">  
This type of graph is often referred to as a *commit graph*. A *commit graph* will be used throughout the tutorial to help you visualize and understand what git does internally after every command.


# Table of contents:
- Starting a Git Project
- Committing
- Branch Workflow and HEAD
- Logging
- Detached HEAD
- Checking Change History
- Undoing Changes
- Remotes

# Starting a Git Project
To create a new git project, you need to initialize git in your project folder. To do this <code>cd</code> into  your project folder and use the following command:
``` 
$ git init
```
You can use the git init command on any project that has not yet been initialized with git. You can tell if a project has been initialized if it contains the *.git* folder. A project that uses git will be called a *git repository* from now on.

So for the calculator program as shown above, the developer would have to execute the following commands:
```    
$ mkdir Calculator && cd Calculator
$ git init
```

# Committing
A *commit* is the technical term for taking a snapshot of your code; that is, git will save the current state of your project. In the commit graph, each of the circles represent a commit. 

Let's say the developer has finished coding the "addition functionality" of the project through coding a file called *addition.txt*. To save or commit the changes to the project, including the changes to *addition.txt*, use:  
(Remember that all lines starting with a **#** are comments)
```
# Commands are: git add .
# and: git commit -m "<message>"
$ git add .
$ git commit -m "Coded addition.txt"
```
The first command, <code>git add .</code>, will be explained in Staging; for now, treat it as a mandatory command to execute before <code>git commit</code>. <code>git commit</code> will take a snapshot of your current code with your custom commit name, which in this case is *Coded addition.txt*, resulting in the first commit on the commit graph.

<img src="addition graph.png" alt="Coded addition.txt commit graph">
  
[//]: <> (After you commit, you will see a message similar to the following:)
```
[master (root-commit) f88cfc7] Coded addition.txt
 1 file changed, 2 insertions(+)
 create mode 100644 addition.txt
```
This is the message you get when you have a successful commit. On the bottom line(s), you can see which files git has saved on this commit.

Sometimes, you find yourself using the <code>git commit</code> again without committing nothing new, you will get the following message:
```
$ git add .
$ git commit -m "No Changes Made"
On branch master
nothing to commit, working directory clean
```


However, this is not to say that you cannot commit after you have already committed once. Assume the developer has decided to commit again after coding the file *subtraction.txt*.
```
$ git add .
$ git commit -m “Coded subtraction.txt”
```
<img src="subtraction graph.png" alt="Coded subtraction.txt commit graph">
Each consecutive commit will have a reference to the previous commit(s), represented with an arrow in the figure above.


Another common mistake is that you commit too early and forget to commit some other changes. To recommit the changes with the last commit you did, use the commits <code>--amend</code> option.

For example, if the developer has decided to recode *subtraction.txt*, he/she will execute the following commands:
```
$ git add .
$ git commit --amend
[master 99f2b6a] Coded subtraction.txt
 Date: Mon Nov 19 16:46:15 2018 -0500
 1 file changed, 2 insertions(+)
 create mode 100644 subtraction.txt
```
The <code>--amend</code> modification saves your work in the same way a normal commit would, however, using <code>--amend</code> will reduce the complexity of the git commit graph. This allows for more efficient git operations as you will see in the future.
<img src="amend graph.png" alt="--amend commit vs a normal commit">

# BRANCH Workflow and HEAD
Branches and HEAD are pointers that is used to organize the commit graph. Adding on these pointers to the commit graph, it would look like the following.
<img src="HEAD and master.png" alt="A complete commit graph with branches and HEAD"> 

## Branch Workflow
A branch is a pointer to a commit. When you start your git project, you are given a default branch called *master*. You can learn how branches work in the *Effectively using Branches* section.
To show how branches work, lets first create a new branch:
```
# git branch <branch_name>
$ git branch GUI      
```
By executing <code> git branch GUI</code>, a *GUI* branch is created at the current commit.
<img src="branch GUI.png" alt="adding on branch GUI to commit graph">

We can then list all our branches using:
```
$ git branch
  GUI
* master
```
The asterisks (\*) by master means that *HEAD* is pointing the master. The variable *HEAD* is used to keep track of which branch you are currently on. This is required as many git operations are done relative to *HEAD*.

Vocabulary: 

The branch your HEAD is pointing to is referred to as the *HEAD branch* and the commit is HEAD is pointing to is referred as the *HEAD commit*.

For example, when we commit only the HEAD branch is affected: The HEAD branch moves forwards with your new commit and the other branches remain unchanged. If the HEAD branch is master and we create a commit with the name "Coded multiplication.txt", the following git graph will emerge. 
<img src="multiplication graph.png" alt="commiting while on the master branch">

Now branch master and branch GUI will point to different commits.

If we were to commit with the GUI branch instead, only the GUI branch will move fowards.
<img src="Coded GUI pt 1.png" alt="commiting with the GUI branch">

Here is a special pattern though. There is now multiple branches of commits. The workplace of the commits in both branches are different and are now completely independent from each other: as in if you change a file in one branch, the files in other branches do not change.

To switch between branches using the command:
```
Git checkout <branch>
```
<img src="checkout graph.png" alt="git checkout commit graph">

Switching to a branch is often referred as *checking out* a branch.

Branches can be delted using:
```
Git branch -d <branch>
```
Deleting branches is dangerous can result in a loss of work, especially if you have multiple lines of commits. For example, if the GUI branch was deleted, the commits can no longer be referenced and therefore they will be deleted. 
<img src="delete GUI graph.png" alt="">

Remember when you delete a branch, you are only deleting the branch pointer. The commits that are deleted is a direct result of deleting this pointer.

You can safely delete a branch as long as another branch can reference the commits.
<img src="delete GUI graph2.png">

**Clarification**

Remember that a branch is only a pointer. Although one of the functions of branches is to allow for commits in parallel, when we say delete a branch, we are only deleting the pointer.

## Workspace when working with branching
Recall that each commit contain different states of code. For example the GUI and master commits may contain the following files.
```
# On GUI
$ ls -1
addition.txt
gui.txt
subtraction.txt

# On master
$ ls -1
addition.txt
multiplication.txt
subtraction.txt
```
Fortunately when working with git, you will only be working the state of the code in one commit, as determined with HEAD. For example, if HEAD points master, it will only have the files resulting from the commits in the master branch.
```
$ ls -1
addition.txt (master version)
multiplication.txt (master version)
subtraction.txt (master version)
``` 
By checking out different branches, you can edit different states of your program simultanously. For example we can work on the master branch without affecting the code state in the GUI branch and work on the GUI branch without affecting the master branch.

## Detached HEAD
We have reviewed that the HEAD variable represents the state of code that you are currently working on, however, the HEAD variable does not have to be a branch.
You also checkout a commit using <code>git checkout \<commit\></code>. This is called the detached HEAD state. This can be used to check or edit code in a certain commit. (see how to reference a commit in the next section)  
<img src="detached HEAD graph.png">  
In the detached state, you can commit, and it will look like such. But once you leave the detached HEAD state, you will need to create a branch or the commits are lost.  
<img src="detached HEAD after commit graph.png">

## How to effectively use branches
Branches are used for two main reasons:
- It allows people to work together on a project without interfering with each other. 
  E.g. If your are working on a new feature, you do not want other developers to commit changes to your branch as it may not be compatible with the code you are working on
- It allows a stable branch and a experiemental branch
  E.g. If there is a feature you want to implement, you can implement it on a new branch to prevent possibly unusable code from polluting your main/stable branch

Git also has a merge feature that combines the change history among multiple branches. However, the merge feature is not perfect and may result in merge conflicts.

To prevent merge conflicts, developers should work on separate aspects of the program. For example, if the two branches that you are trying to merge both modifies the same text file in different ways, such as below, it will result in the following merge conflicts.
```
# will put them side by side later
Branch1
def addition(a, b):
  return a + b

PI = 3.14
E = 2.72

def additionBinary(a, b):
  return binaryAdd(toBinary(a), toBinary(b))

Branch2
def addition(a, b):
  return a + b


PI = 3.141592
E = 2.7182818

def additionBinary(a, b):
  return toBinary(a + b) 

def additionHex(a, b):
  return toHex(a + b)

conflicts
def addition(a, b):
  return a + b

<<<<<<< HEAD
PI = 3.14
E = 2.72

def additionBinary(a, b):
  return binaryAdd(toBinary(a), toBinary(b))
=======

PI = 3.141592
E = 2.7182818

def additionBinary(a, b):
  return toBinary(a + b) 
>>>>>>> branch2  
```
However, if the code had been coded in two files with different names, there would not have been a merge conflict.


## MERGING BRANCHES
When you merge, you use the command:
```
git merge <branch> <branch2> <branch3> ….
```
This merges the given branches with the HEAD branch (given that you are not in an detached HEAD state).

For the merge examples, we will assume that the project has a commit graph as such:
<img src="original merge graph.png">

There are two types of merges:

**Merging single branch of commits**

The git merge merges the HEAD branch with the given branch. In this case, one branch is ahead of the other branch, so git only will move the branch forwards. This is called fast-forwarding. There will not be any merge conflicts.
```
$ git checkout master       # change HEAD branch to Master
Switched to branch 'master'
$ git merge Math 
Updating 99f2b6a..b75dc55
Fast-forward
 division.txt       | 3 +++
 multiplication.txt | 2 ++
 2 files changed, 5 insertions(+)
 create mode 100644 division.txt
 create mode 100644 multiplication.txt
```
<img src="merge Math graph.png">

**Merging multiple branches of commits**
Git will create a new snapshot that results in this merge and moves the branch pointer forward. This commit is called a merge commit and is special in that it has two parents.

```
$ git branch            # HEAD branch is master again as shwon from asterisks (*)
  GUI
* master
$ git merge GUI
Merge made by the 'recursive' strategy.
 gui.txt | 6 ++++++
 1 file changed, 6 insertions(+)
 create mode 100644 gui.txt
```

<img src="merge GUI graph.png">
As said earlier, merging result in merge conflicts. Usually git will show conflicts instead and make it obvious where and in which files merging when wrong. You need to fix the conflicts, and manually commit again to create the merge commit. Git status tells you where the conflicts are. So for the previous example addition.txt conflict, the git status output would be the following:

```
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:   addition.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
It is very rare for git to actually make your program unrecoverable after a merge.


**Rebasing**

Not as commonly used, but Git has another merging function called git rebase. Git rebase will reapply all changes made in one branch and reapply it into another branch. While rebasing has no merge conflicts, it does not guarantee that the end result is a stable program. Also, it removes all the original commits along with the rebased branch, as there are no longer any branches referencing them. If you would like to save those commits, you need to create another branch on that commit. Unfortunately you cannot rebase multiple branches.

**Aborting Merge**

If you have merge conflicts and would no longer want to deal with the merge conflicts, use one of the following two commands (depending on which merge algorithm you use).
<code>git merge -abort
git rebase –abort</code>


## Viewing commit history
You can check your commit history using the git log command.
By default, git log lists the commits made in that repository in reverse chronological order.
For each commit, you can see the commit ID, the the author’s name and email, the date written, and the commit message. The commit ID will look longer than the commit ID that was shown as you commited your program, even though they are the same, as you do not need the whole ID to uniquely identify a commit.
There are several options you can use with git log:
    --reverse show the commit history in chronological order (instead of reverse chronological order)
    -p Shows the changes introduced in each commits

```
$ git log
commit 0adc831ee0c111fa9c95b29383cf5cff3677494f
Merge: b75dc55 91b994c
Author: frankliu197 <frankliu197@gmail.com>
Date:   Mon Nov 19 17:06:29 2018 -0500

    Merge branch 'GUI'

commit 91b994c9a138726f8c861b9cb1947bc6eddb8f3d
Author: frankliu197 <frankliu197@gmail.com>
Date:   Mon Nov 19 16:58:12 2018 -0500

    Coded GUI pt.2

# next few lines of output are cut
```

If you want to see the commit graph of your project, use the following command.
  git log --all --decorate --oneline –graph



## Referencing
Various commands require parameters, as shown with the angled brackets (<>) throughout the tutorial. For example, \<commit\> would require a commit parameter. You can use one of the listed methods to pass a parameter.

**Referencing commits**
The easiest ways to reference a commit are:
- Using the commit Id (as shown in git log)
- HEAD (for the current commit only)
In most cases, you can also use a branch, since a branch is a reference to a commit. However, this is not true if the command performs different actions when given a branch and commit parameter (such as in the case of <code>git checkout</code>).
<img src="referencing commits.png">

Every commit can be represented with some sort of combination relative to HEAD
- HEAD~#, where # represents the number of commits up the hierarchy, via the first commit if there is more than one commit
- HEAD^#, goes up one commit, relative to the # parent if there is more than one commit
Notes:  The symbols ^ and ~ can be combined
        You do not need to write # if # is 1 (e.g. HEAD\~1 is the same as HEAD\~)
<img src="HEAD graph.png">

**Referencing files**

With git, you can reference files with all wildcard symbols (such as * or .) used in terminal. (See shell regex)

```
# the wildcard * means all files
$ git add * 
# tells git to do the add function to all files
```

**Referencing branch**

You can reference a branch only with the branch name. You cannot use the variable HEAD as HEAD is a pointer to your current workspace and is not a branch.



## Tracking and Staging:
Git gives various states to files/changes in your git repository.

**Tracked and Untracked:**
Each file in a git repository is given a status tracked and untracked:
A tracked file means git will keep track of the changes given on the file.
An untracked file means that git is unaware of its existence. In this case, git will not track any changes given to the file and will not perform any functions to the file (such as commit) until you tell git to track it.

Untracking is useful for if you have system dependent files (such as cache and ssh files) that you do not want to save with the project.


**Staging and Unstaging:**

Git tracks all changes in tracked files as one of two statuses: staged and unstaged :

Staged changes are batched to be saved the next time git performs a commit, and will be part of your changed history.

Unstaged changes are changes where git will not save on the next commit, and will not be saved in your change history.

Staging is used so you can choose which changes to save in a certain commit.

<img src="commit with staging.png">
**Working with tracking and staging**
By default, a file is untracked, and all changes are unstaged. So when you first create a file, any changes made to the file is not recognized by git. To let git track your file, you need to run <code>git add \<file\></code>. With a tracked file, when you make changes to the file, the changes are unstaged. To stage the changes for a commit, use the command: <code>git add Nfile></code>.

Thus the command to both track a file and to stage all changes in a file is: <code>git add \<file\></code>
Because the command to track and stage files is the same, when you use <code>git add \<file\></code> on an untracked file, the file will both be tracked and its contents will be staged.

<img src="add and staging.png">

This is why previously we have always executed <code>git add .</code> before commiting, where the period (.)  means all changes in the current folder (and subfolders).



You can see what is staged, unstaged and untracked using git status. There is also a -s modifier to give you a condensed output.
```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

  modified:   addition.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

  modified:   addition.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

  gui.txt
```

In git status, a file can appear in both the staged and unstaged section.
When you use <code>git add \<file\></code>, all changes that were done up to the moment the command was executed are staged. All future changes to the file will be unstaged until you run <code> git add <file> </code> again.

<img src="staging while editing.png">

If you want to unstage all changes in a file from the last commit, use <code>git reset HEAD \<file\></code>. You can unstage all changes from all files using using <code>git reset HEAD</code>.

To untrack a tracked file use <code>git remove –cache \<file\></code>.

<img src="staging states.png">

**Git Ignore**

You can tell git to ignore some files by listing them in a file called .gitignore. These files will not be tracked, and they will not be included in commands with the all modifier (e.g. git add -A). The .gitignore file is not created by default, and it is a good idea to add it to the base folder of your git repository.
```
# .gitignore file
# Not allowed to use tabs before the file name
gui/\*          # ignore all files from gui folder
!gui/gui.txt    # except for the file gui/gui.txt

temp.txt        # ignore a file called temp.txt
````
Similarly in the .gitignore, you can tell git not to ignore some patterns using “!”.
If you want git to ignore an already tracked file, you will have to untrack the file first.

**Git keep**

By default git only tracks files. Therefore all folders are untracked unless the folder contains a tracked file. Thus if you really want to commit a folder without a tracked file, it is a common practise to create a tracked file called *.gitkeep* inside the folder.

## Check change history
You can check your change history to your project using git diff.
By default git diff shows you all unstaged changes; it tells you what has not been added for commiting. Here are some modifiers you can use with git diff:
--staged Shows you all your the staged changes instead

<code>git diff \<commit\></code> shows you all changes done to the git repository since the given commit.
Thus you can see all staged and unstaged changes together since your last commit with: <code>git diff HEAD</code>

```
$ git diff HEAD
diff --git a/gui.txt b/gui.txt
index 6fe99a7..e548684 100644
--- a/gui.txt
+++ b/gui.txt
@@ -4,3 +4,5 @@ def showGUI():
 def hideGUI():
   hide()
 
+def makeGUI():
+  //code
```

<img src="diff states.png">

## Undoing Changes
There are several git commands that allow you to undo changes both individually or as a whole.

**Git clean:**

Git clean removes all untracked files excluding the files listed in .gitignore. This is good to remove the files you have added since the previous commit.

There are several options you can use with git clean:  
    -n This is a dryrun, you can see what files will be deleted without actually removing any files  
    -x remove files included from gitignore in addition to the untracked files  
    -X remove only files from gitignore  
    -f force clean if git refuses to clean the untracked files  

For example, if you had an untracked file named subtractionTester:
```
$ git clean
Removing subtractionTester.txt
```

**git checkout**

Normally the checkout command is for changing the HEAD pointer. However you can also use the command:
<code>git checkout \<commit\> -- \<file\></code>, to get specific versions of a file from another commit (including from other branches).

For example, (show an example by getting a file from another branch)/

Checkout is commonly used to undo all changes to a file since the last commit using:
```
$ git checkout HEAD -- <file>
# For example:
# git checkout HEAD – bin/* undoes all changes from files in the bin directory.
```
The files changed by git checkout are automatically staged.

**git reset**
Git reset moves back the HEAD branch to reference the specified commit. The commits between the previous HEAD commit and the specified commit are lost, and the change history in those commits are “reset”. For example, if you use git reset A, the commits B and C are effected.

<img src="reset graph.png">

There are three different ways you can use <code> git reset <commit> </code>:  
<img src="reset options.png">  
<img src="reset states.png">
For example, you can remove unwanted changes from the last commit using git reset –hard HEAD.


**git revert**

Git revert will undo changes from any previous commit, and creates a new commit with the changes undoed.
```
$ git revert <commit>
```
<img src="revert graph.png"> 
This is different from reset in that it can undo any one commit without undoing the more recent commits. As well it does not remove any commits so it is easily revertable.

**Remotes**
    As you work on code, you must share your changes with other people To share code online, you will use use Remotes. Remotes are pointers to online repositories, such as Github, Gitlab and Heroku. We will use Github.
After first creating a repository on github, we can set up an online repository with the git repository with the following command:
    git remote add <name_of_remote> <repository URL>
    e.g. git remote add origin https://github.com/A/Calculator
In this case, our remote is called origin and our repository URL github.com/A/Calculator. Typically the name of your main remote is called Origin. Now to post your code onto your remote, use the command:
    git push <remote> <branch>
    e.g. git push origin master
    So if this project was coded by two people and developer A has started the project, developer B has no access to the code. After all the code is in developer’s A local hard drive. To share this code, A has to pushed his code to the repository, so B can download and start editing the project.
    For B to download the project, the following commands are used
    git clone <URL>
    git clone https://github.com/A/Calculator
This will create a new git project on your local computer inside a new folder called Calculator, and then copy all the code in that repository. Since git clone DOWNLOADS the whole project from the repository into a new folder, you will only use git clone the first time you download the project. After that, if you want to receive the latest changes from online you will use one of the following:
    git fetch <remote>
    git pull <remote>
git fetch will update your changes into a new branch, and git pull will auto merge the code in the repository with your branches. In other words, git fetch = git pull + git merge.
    Thus whenever whenever a new version of code is pushed into origin, the commands: git fetch/pull origin are needed to download it.

Working simultaneously:
    With remotes, it is much easier for two people on same version of the project at once. It seems counterproductive for two people to code the same aspect of project at once, but if the people work on separate parts of the project and use git merge lots of progress can be done. For example if developer A worked on the Math aspect and developerB worked on the GUI aspect separately, they will be able to merge thier work perfectly without any conflicts.

Managing Remotes:
The location of the remotes are stored in the .git folder. So after the repository has been set by one person, it does not have to be set again. You can check all the set remotes using:
    remote -v

You can reset the name of the url or rename the remote using:
git remote set-url <remote> <new_URL>
git remote rename <old_remote> <new_remote>

Or you can delete the remote with:
git remote rm <remote>

Remote branches (need to test later)
    What are they
    How to use them
    merging them
    --tracking/automerge
https://git-scm.com/book/id/v2/Git-Branching-Remote-Branches

Stashing

Tagging

Conclusion
Throughout this guide you have learnt both basic git usage as well as some more advanced git functions, which is much more than enough to code most projects. If there is any command you do not quite understand, you can use the following command:
git help <action>
There will be a cheat sheet coming soon.

Now that you know how to use git, it is a good idea to check out what you can do with git on the web, such as push and pull requests and continuous integration!



coming soon
STASHING https://www.atlassian.com/git/tutorials/saving-changes/git-stash
https://medium.freecodecamp.org/useful-tricks-you-might-not-know-about-git-stash-e8a9490f0a1a

TAGGING

CherryPicking

