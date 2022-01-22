
<h1> How to use git </h1>

NOTE: THIS IS IN BETA   
This will not reflect my true writing skills. Check out https://github.com/frankliu197/Linux-Tutorial to see completed (and how i truly) write technical articles
## Prerequisite knowledge:
* Basic Terminal Commands and Usage (Tutorial Here)
* Understanding of basic programming concepts (such as Variables and Pointers) 
* General project workflow

If you have been programming for some time, you certainly would have heard about git. Git is a version control software that can be used to greatly simplify
the process of developing software. Some benefits of using a version control software include:
- Reverting your code to a previous states if something goes wrong
- Multiple developers can work on ONE project without interfering with each other

This tutorial will concisely explain the usage of the common (as well as the not so common) git commands on the Linux terminal. 
The example project that we will code with git is a calculator and its changes can be visualized as such:
<img src="calculator graph.png" alt="Calculator Commit Graph">  
This type of graph is often referred to as a *commit graph*. These commit graphs will be explained in further detail throughout the tutorial.


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
You can use the git init command on any project that has not yet been initialized with git. You can tell if a project has been initialized if it contains the *.git* folder.
 A project that uses git will be called a *git repository* from now on.

So for the calculator program as shown above, the you would have to execute the following commands:
```    
$ mkdir Calculator && cd Calculator
$ git init
```

Summary:
```
Create a new repository: git init
```

# Committing
Whenever you *commit* with git, you save the current state of your code.  

Let's say when coding the calculator project, you have coded a called *addition.txt* which contains the addition functionality of the calculator.
To save or commit the changes to the project, including the changes to *addition.txt*, use:  
(Remember that all lines starting with a **#** are comments)
```
# Commands are: git add .
# and: git commit -m "<message>"
$ git add .
$ git commit -m "Coded addition.txt"
[master (root-commit) f88cfc7] Coded addition.txt
 1 file changed, 2 insertions(+)
 create mode 100644 addition.txt
```
The first command, <code>git add .</code>, will be explained in Staging; for now, treat it as a mandatory command to execute before <code>git commit</code>.
<code>git commit</code> will take a snapshot of your current code with your custom commit name, which in this case is *Coded addition.txt*, resulting in the first commit on the commit graph.
<img src="addition graph.png" alt="Coded addition.txt commit graph">
The commit is represented in a circle in the graph above. It is best to think of the commit as a saved state of your project.

After you finish commiting, you can continue to edit your project. If you continue to edit the project, the current state of the project will be different from the state saved in your most recent commit.
Say you have then coded  *subtraction.txt*. The state of your current project and the state of your commit are different as shown below.
<img src="after addition graph.png">

To save those new changes, you will need to commit again.
```
$ git add .
$ git commit -m “Coded subtraction.txt”
[master 99f2b6a] Coded subtraction.txt
 Date: Mon Nov 19 16:46:00 2018 -0500
 1 file changed, 2 insertions(+)
 create mode 100644 subtraction.txt
```
<img src="subtraction graph.png" alt="Coded subtraction.txt commit graph">
Remember that each commit is a saved state of your project. Therefore the commits "Coded addition.txt" and "Coded substraction.txt" represents different states of your project. 

Often, you may commit too early and forget to commit some other changes. To recommit the changes with the last commit you did, use the commits <code>--amend</code> option.

For example, if you decided to recode *subtraction.txt*, you can recommit using the <code>--amend</code> option.
```
$ git add .
$ git commit --amend
[master 99f2b6a] Coded subtraction.txt
 Date: Mon Nov 19 16:46:15 2018 -0500
 1 file changed, 2 insertions(+)
 create mode 100644 subtraction.txt
```
The <code>--amend</code> modification saves your work in the same way a normal commit would, but provides several benefits over a normal commit.
<img src="amend graph.png" alt="--amend commit vs a normal commit">
While the commits "Coded addition.txt" and "Coded subtraction.txt" represent the same state of code, using <code>--amend</code> will reduce the complexity
of the commit graph. This allows for more efficient git operations as you will see in the future.

Another common mistake you may make is to commit nothing.
```
$ git add .
$ git commit -m "No Changes Made"
On branch master
nothing to commit, working directory clean
```
Fortunately in this case git will not create a new commit as there is "nothing to commit". Your commit graph will remain the same.

```
To commit: git commit -m "<message>"
To recommit your last commit: git commit --amend
```

# BRANCH Workflow and HEAD
Branches and HEAD are pointers that is used to organize the commit graph. Adding on these pointers to the commit graph, it would look like the following:  
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
The asterisks (\*) by master means that *HEAD* is pointing the master. The variable *HEAD* is used to keep track of which branch you are currently on.
This is required as many git operations are done relative to *HEAD*.

**Vocabulary:**
The branch your HEAD is pointing to is referred to as the *HEAD branch* and the commit the HEAD is pointing to is referred as the *HEAD commit*.

When we commit, only the HEAD branch is affected: The HEAD branch moves forwards with your new commit and the other branches remain unchanged.
If the HEAD branch is master and we create a commit with the name "Coded multiplication.txt", the following git graph will emerge.   
<img src="multiplication graph.png" alt="commiting while on the master branch">
Now the master and GUI branch point to different commits, or different states of your code. 

Instead, you were to commit with the GUI branch instead, only the GUI branch will move forwards.  
<img src="gui pt 1.png" alt="commiting with the GUI branch">  
In this graph, the commits will diverge. In this case, the code in either branches are independent from each other:
as in if you change a file in one branch, the files in other branches do not change. 

And just like before, if you were to code in a branch, the code in other branches remains unaffected. 
<img src="independent commits.png">
Remember that each time you commit, you add new state of your project onto the commit graph. Thus state of the commits "Coded Multiplication" and "Coded GUI pt 1" remains the same as well.
**Switching between branches**
You can switch between branches using the command:
```
Git checkout <branch>
```
<img src="checkout graph.png" alt="git checkout commit graph">

Switching to a branch is often referred as *checking out* a branch.

**Deleting branches**
Branches can be delted using:
```
Git branch -d <branch>
```
Deleting branches is dangerous can result in a loss of work, especially if you have multiple lines of commits. For example,
if the GUI branch was deleted, the commits can no longer be referenced and therefore they will be deleted. 
<img src="delete GUI graph.png" alt="">

Remember when you delete a branch, you are only deleting the branch pointer. The commits that are deleted is a result of deleting this pointer.

You can safely delete a branch as long as another branch can reference the commits.
<img src="delete GUI graph2.png">

## Workspace when working with branching
Recall that each commit contain different states of code. As we can see the workplace in master and GUI contain the following files.
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
Fortunately when working with git, you will only be working the state of the code in one commit, as determined with HEAD. For example, 
if HEAD points master, it will only have the files resulting from the commits in the master branch.
```
$ ls -1
addition.txt (master version)
multiplication.txt (master version)
subtraction.txt (master version)
``` 
By checking out different branches, you can edit different states of your program simultanously. 

## How to effectively use branches
Branches are used for two main reasons:
- It allows people to work together on a project without interfering with each other. 
  E.g. If your are working on a new feature, you do not want other developers to commit changes to your branch as it may not be compatible with the code you are working on
- It allows a stable branch and a experiemental branch
  E.g. If there is a feature you want to implement, you can implement it on a new branch to prevent possibly unusable code from polluting your main/stable branch

Git also has a merge feature that combines the change history among multiple branches. However, the merge feature is not perfect and may result in merge conflicts.

To prevent merge conflicts, developers should work on separate aspects of the program. For example, if the two branches that you are trying to merge 
both modifies the same text file in different ways, such as below, it will result in the following merge conflicts.
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
$ git merge <branch> [branch2] [branch3]
# where square brackets [] represent optional parameters
```
This merges the given branches with the HEAD branch (given that you are not in an detached HEAD state).

For the merge examples, we will assume that the project has a commit graph as such:
<img src="original merge graph.png">

There are two types of merges:

**Merging single branch of commits**

The git merge merges the HEAD branch with the given branch. In this case, one branch is ahead of the other branch, so git only will move the branch forwards.
This is called fast-forwarding. There will not be any merge conflicts.
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
As said earlier, merging result in merge conflicts. Usually git will show conflicts instead and make it obvious where and in which files merging when wrong.
You need to fix the conflicts, and manually commit again to create the merge commit. Git status tells you where the conflicts are. So for the previous example
addition.txt conflict, the git status output would be the following:

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
Git rebase works the same way has git merge except that it deletes the branch that is being merged to the HEAD branch.
For example, <code> git rebase GUI </code> has the same effect as <code> git merge GUI && git branch -d GUI </code>  
<img src="rebase graph.png">

You cannot rebase multiple branches.

**Aborting Merge**

If you have merge conflicts and would no longer want to deal with the merge conflicts, use one of the following two commands (depending on the type of merge you use).
```
git merge --abort
git rebase --abort
```

## Detached HEAD
You do not need to always checkout a branch. You can also checkout a commit using <code>git checkout \<commit\></code>. This is called the detached HEAD state.
This can be used to check or edit code in a certain commit. (see how to reference a commit in the next section)  
<img src="detached HEAD graph.png">  
In the detached state, you can commit, and it will look like such. But once you leave the detached HEAD state, you will need to create a branch or the commits are lost (see deleting branches).  
<img src="detached HEAD after commit graph.png">

Summary:
```
List branches: git branch
Create branch: git branch <branch>
Delete branch: git branch -d <branch>
Checkout a state of your code: git checkout <branch/commit>
Merge branch with HEAD branch: git merge <branch1> [branch2] [branch3] [etc...]
Rebase branch with HEAD branch: git rebase <branch>
See merge conflicts: git status
Abort merge/rebase: git merge/abort <branch>
```

## Viewing commit history
You can check your commit history using the git log command.
By default, git log lists the commits made in that repository in reverse chronological order.
For each commit, you can see the commit ID, the the author’s name and email, the date written, and the commit message. There are several options you can use with git log:   
    --reverse show the commit history in chronological order (instead of reverse chronological order)   
    -p Shows the changes introduced in each commits   
    --online Displays a commit in one line

```
$ git log
commit 0adc831ee0c111fa9c95b29383cf5cff3677494f     #Commit ID shown here
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
With the <code>--oneline</code> parameter, you will only see a tiny portion of the commit ID instead. These two values are identical as you do not need the whole ID to uniquely identify a commit.
```
$ git log --oneline
0adc83 Merge branch 'GUI'
91b994 Coded GUI pt.2
# next few lines of output are cut
```
Thus the commit id *0adc83* from "Merge branch 'GUI'" will be complete equivalent to *0adc831ee0c111fa9c95b29383cf5cff3677494f*. The commit ID will be important in the next section. 

If you want to see the commit graph of your project, use <code>git log --all --decorate --oneline –graph</code>

Summary:
```
View commits: git log
  -p Shows changes introduced in each commit
  --reverse Shows the commit history in reverse order
  --oneline Displays each commit in one line
View commit graph: git log --all --decorate --oneline --graph
```

## Referencing
**Conventions**  
Various commands require parameters, as shown with angled brackets \<\> or square brackets [] throughout the tutorial.  
In this tutorial different representation of parameters mean:
\<param\> - Represents a mandatory parameter
   e.g. <code> git checkout \<commit\> </code>
\[param\] - Represents an optional parameter 
\[param=?] - If no param is given, the parameter will have a default value of *?*
  e.g. <code> git reset \[file=.\] </code>
  In this case option will have a default value of *.* if no option parameter is given, where a period in shell regex represents this dictory (And everything inside the directory)
\[param|default=value] - If no param is given, the default action is given with *value* (But this value is NOT a proper argument to the command)
  e.g. <code> git reset [file|default=all] </code>, would mean the same as  <code> git reset \[file=.\] </code>


You can use one of the listed methods to pass a parameter.

**Commit Parameters**   
The easiest ways to reference a commit are:
- Using the commit Id (as shown in git log)
- HEAD (for the current commit only)
In most cases, you can also use a branch, since a branch is a reference to a commit. However, this is not true if the command performs different actions when given a 
branch and commit parameter (such as in the case of <code>git checkout</code>).
<img src="referencing commits.png">

Every commit can be also represented with some sort of combination relative to HEAD
- HEAD~#, where # represents the number of commits up the hierarchy, via the first commit if there is more than one commit
- HEAD^#, goes up one commit, relative to the # parent if there is more than one commit  
Notes:  The symbols ^ and ~ can be combined   
        You do not need to write # if # is 1 (e.g. HEAD\~1 is the same as HEAD\~)  
<img src="HEAD graph.png">  

**File Parameters**

With git, you can reference files with the normal symbols used in terminal (such as * or .). (See shell regex)

```
# the wildcard * means all files, and . means this directory (along with the files in this directory)
$ git add * 
$ git add . 
# tells git to do the add function to all files
```

**Branch Parameters**
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
By default, a file is untracked, and all changes are unstaged. So when you first create a file, any changes made to the file is not recognized by git. 
The command to both track and stage a file is <code>git add \<file\></code>.
Whenever you execute <code>git add</code> on a file, it will track the file (if it hasn't been tracked) and stage its contents.

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

In git status, a file can appear in both the staged and unstaged section. This is because when you use <code>git add \<file\></code>, the files contents at that moment are
staged. All subsequent changes to the file will be unstaged until you run <code> git add \<file\> </code> again.

<img src="staging while editing.png">

If you want to unstage all changes in a file from the last commit, use <code>git reset HEAD [file=all]</code>. You can unstage all changes from all files using using <code>git reset HEAD</code>.

To untrack a tracked file use <code>git remove –cache \<file\></code>.

<img src="staging states.png">

**Git Ignore**

You can tell git to ignore some files by listing them in a file called *.gitignore*. These files will be ignored from git operations such as <code>git add</code> unless specified otherwise. 
The *.gitignore* file is not created by default, and it is a good idea to add it to the base folder of your git repository.
```
# .gitignore file
# Not allowed to use tabs before the file name

# You can list files normally or with terminal regex
temp.txt        # ignore a file called temp.txt
gui/*           # ignore all files from gui folder
!gui/gui.txt    # except for the file gui/gui.txt (the exclamation mark is the negate operator for gitignore)
```
If you want git to ignore an already tracked file, you will have to untrack the file first.

**Git keep**  
By default git only tracks files. Therefore all folders are untracked unless the folder contains a tracked file. THus if you need to track a folder, you will need to add a 
tracked file in the folder. It is a common practise name this file  *.gitkeep*.

Summary:
```
To track and stage a file: git add <file>
To unstage a file: git reset HEAD [file=.]
To untrack a file: git remove --cache <file>
To see staged, unstaged and untracked files: git status [-s]
To ignore a file: Place a reference to the file in .gitignore
To track a folder with no tracked files: Create tracked file in that folder and call it .gitkeep
```

## Check change history  
You can check your change history to your project using <code>git diff</code>.

git diff [--unstaged] Shows you all unstaged changes since last commit
git diff --staged Shows all changes staged changes since last commit
git diff \<commit\> Show all changes done to the git repository since the given commit.  
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

You can also show the difference between any two branches or commits
git diff \<branch1\>..\<branch2\> 
git diff \<commit1\> \<commit2\>

Summary:
```
Check the change history of your project relative to your current workspace:
git diff [--unstaged]
git diff --staged
git diff <commit>

Check the difference between two branches or commits:
git diff <branch1>..<branch2> 
git diff <commit1> <commit2>
```

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

**Git checkout:**

Normally the checkout command is for changing the HEAD pointer. However you can also use the command:
<code>git checkout \<commit\> -- \<file\></code>, to get specific versions of a file from another commit (including from other branches).

Checkout is commonly used to undo all changes to a file since the last commit.

For example, the following command undoes all changes from files in the bin directory since the most rececnt commit.
```
$ git checkout HEAD – bin/* 
```
The files changed by git checkout are automatically staged.  

**git reset**
Git reset moves back the HEAD branch to reference the specified commit. The commits between the previous HEAD commit and the specified commit are lost, 
and the change history in those commits are “reset”. For example, if you use git reset A, the commits B and C are effected.

<img src="reset graph.png">

There are three different ways you can use <code> git reset \<commit\> </code>:  
<img src="reset options.png">  
<img src="reset states.png">
For example, you can remove unwanted changes from the last commit using <code>git reset –hard HEAD</code>.


**git revert**  
Git revert will undo changes from any previous commit, and creates a new commit with the changes undoed.
```
$ git revert <commit>
```
<img src="revert graph.png"> 
This is different from reset in that it can undo any one commit without undoing the more recent commits. As well it does not remove any commits so it is easily revertable.  

Summary:
```
Remove all untracked (but not ignored files): git clean
    -n show files will be deleted without actually removing any files  
    -x remove files included from gitignore in addition to the untracked files  
    -X remove only files from gitignore  
    -f force clean  
Checkout a file from another commit: git checkout \<commit\> -- \<file\>
Undo changes to the given commit: git reset [default=--mixed] <commit>
  --soft all affected changes are staged
  --mixed all affected changes are unstaged
  --hard all affected changes are deleted
Create a new commit undoing changes from any previous commit: git revert <commit>
```

## Remotes  
As you work on code, you must share your changes with other people To share code online, you will use use Remotes. Remotes are pointers to online repositories, such as Github, Gitlab and Heroku. We will use Github.  
After first creating a repository on github, we can set up an online repository with the git repository with the following command:  
```
$ git remote add <name_of_remote> <repository URL>
e.g. git remote add origin https://github.com/A/Calculator
```
In this case, our remote is called origin and our repository URL github.com/A/Calculator. Typically the name of your main remote is called Origin. Now to post your code onto your remote, use the command:   
```
$ git push <remote> <branch>
e.g. git push origin master
```
So if this project was coded by two people and developer A has started the project, developer B has no access to the code. After all the code is in developer’s A local hard drive. To share this code, A has to pushed his code to the repository, so B can download and start editing the project.  
For B to download the project, the following commands are used:  
```
$ git clone <URL>
e.g. git clone https://github.com/A/Calculator
```

This will create a new git project on your local computer inside a new folder called Calculator, and then copy all the code in that repository. Since git clone DOWNLOADS the whole project from the repository into a new folder, you will only use git clone the first time you download the project. After that, if you want to receive the latest changes from online you will use one of the following:  
`$ git fetch <remote>`  
`$ git pull <remote>`


```git fetch``` will update your changes into a new branch, and git pull will auto merge the code in the repository with your branches. In other words, ```git fetch = git pull + git merge```. Thus whenever whenever a new version of code is pushed into origin, the commands: git fetch/pull origin are needed to download it.  

With remotes, it is much easier for two people on same version of the project at once. It seems counterproductive for two people to code the same aspect of project at once, but if the people work on separate parts of the project and use git merge lots of progress can be done. For example if developer A worked on the Math aspect and developerB worked on the GUI aspect separately, they will be able to merge thier work perfectly without any conflicts.  

**Managing Remotes:**  
The location of the remotes are stored in the `.git` folder. So after the repository has been set by one person, it does not have to be set again. You can check all the set remotes using:  
```$ remote -v```

You can reset the name of the url or rename the remote using:  
```$ git remote set-url <remote> <new_URL>```
```$ git remote rename <old_remote> <new_remote>```

Or you can delete the remote with:  
```$ git remote rm <remote>```  

**Summary:**
```
Clone a repo: git clone <URL>
Fetch updates: git fetch <remote> [branch|default=all]
Fetch and merge updates: git fetch <remote> [branch|default=all]
Push updates: git push <remote> <branch>
Push all branches: git push --all <remote> 
Add remote: git remote add <remote_name> <URL>
List remotes: git remote -v
Remove remote: git remote rm <remote>
Re-set remote URL: git remote set-url <remote> <new_URL>
Show remote details: git remote show <remote>
Rename remote: git remote rename <old_remote_name> <new_remote_name>
```


To be written later
Remote branches

Summary:
```
List remote branches: git branch -a
Delete remote branch: git push origin --delete <remote_branch>
```

Tagging


Stashing

Cherrypicking
