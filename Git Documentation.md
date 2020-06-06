
Q] I can use Dropbox, Google Drive, OneDrive, a flash drive… any other drive to store my code and share it with everyone!
Ans: That's true. But again, how can you know for sure which version of code is the latest? And if your friend wants to merge your code with his version in the same file, would you like to go line by line to see whose code will be merged into which part of a file? We bet you wouldn't.
Git easily solves both problems we've described: managing project versions and sharing code among developers. But to make Git our best friend, we should understand how Git works. To do so, we should also start using basic Git commands.

Types of version control
When you work on a development team, you may be touching similar parts of the code throughout a project. As a result, changes made in one part of the source can be incompatible with those made by another developer working at the same time.
Version control helps solve these kinds of problems and provides:
	A complete history of every file, which enables you to go back to previous versions to analyze the source of bugs and fix problems in older versions.
	The ability to work on independent streams of changes, which allows you to merge that work back together and verify that your changes conflict.
	The ability to trace each change with a message describing the purpose and intent of the change and connect it to project management and bug tracking software.
There are two types of version control: centralized and distributed.
Centralized version control
With centralized version control systems, you have a single “central” copy of your project on a server and commit your changes to this central copy. You pull the files that you need, but you never have a full copy of your project locally. Some of the most common version control systems are centralized, including Subversion (SVN) and Perforce.
Distributed version control
With distributed version control systems (DVCS), you don't rely on a central server to store all the versions of a project’s files. Instead, you clone a copy of a repository locally so that you have the full history of the project. Two common distributed version control systems are Git and Mercurial.
While you don't have to have a central repository for your files, you may want one "central" place to keep your code so that you can share and collaborate on your project with others. That's where Bitbucket comes in. Keep a copy of your code in a repository on Bitbucket so that you and your teammates can use Git or Mercurial locally and to push and pull code.



What is Git?
Ans: Git is a distributed version control system (DVCS). "Distributed" means that all developers within a team have a complete version of the project. A version control system is simply software that lets you effectively manage application versions. Thanks to Git, you'll be able to do the following:
•	Keep track of all files in a project
•	Record any changes to project files
•	Restore previous versions of files
•	Compare and analyze code
•	Merge code from different computers and different team members.
These capabilities listed above don't tell how Git actually works, however. In all its complexity, Git works quite simply: you first need to create a local repository in your project's root directory (folder). Afterwards, Git can track project files and directories and add them to the repository. You may be thinking
What are the ways for executing?
Ans: The Ways:
	Git Bash
	Git SVN
Git Architecture
 

Adding a file to the Staging Area.
 To stage a file is simply to prepare it finely for a commit. Git, with its index allows you to commit only certain parts of the changes you've done since the last commit.
Git has three main states that your files can reside in:
a)	Commited: Committed means that the data is safely stored in your local database. 
b)	Modified: Modified means that you have changed the file but have not committed it to your database yet.
c)	Staged: Stagedmeans that you have marked a modified file in its current version to go into your next commit snapshot.

Commands:
A.	Git Configuration
	$ git config - -global user.name “King”
	$git config - -global user.email “king@gmail.com” 
	$git config - -list


B.	Starting a new local repository with git
	$git init    //Creating a local repository
	$git status  //To know git status

C.	Staging Files:

	$git add <file-name>
	$git add <file-name> <another-file-name> <yet-another-file-name>
	$git add.    //For commiting All Files in the Staging Area, but it will only add files located in the root directory
	$git add - - all   //Find all new and updated files everywhere throughout the project and add them to the staging area.
	$git add – A    // Same as above
	$git rm - - cached <file-name>  //For Unstage a file
	$git reset <file-name>        //exact opposite of add

D.	Commiting to a repository

	$git commit -m “Initial Commit” //move (commit) files from the staging area (an imaginary basket) to the repository (a lock box).//-m stands for message
	$git commit -a -m “Initial Commit //Add modified files to the Staging Area and commit them at the same time. //-a stands for add 
	//Let's say you've modified ten files but committed only nine. How can you add that remaining file to the last commit? And how can you modify a file if you've already committed it?
Two Ways:
1.	Undo Commit
      $git reset –soft HEAD^ //"reset" tells Git to undo the commit. The "--soft" option means that the commit is canceled and moved before HEAD. You can now add another file to the staging area and commit, or you can amend files and commit them.

To understand what that "HEAD" thing represents, recall that we work in branches. Currently we're in the master branch, and HEAD points to this master branch. When we switch to a different branch later, HEAD will point to that different branch. HEAD is just a pointer to a branch:
 
What you see in the image is that each dot represents a separate commit, and the latest commit is at the top of the branch (HEAD). In the command "git reset --soft HEAD^" the last character "^" represents the last commit. We can read "git reset --soft HEAD^" as "Undo the last commit in the current branch and move HEAD back by one commit."
2.	Instead of resetting the HEAD and undoing the last commit,
$git commit - -ammend -m <enter your message> //Adding Remaining Files into the Staging Area and then Commit

E.	Pulling and Pushing from and to repositories

Approach A]
	$git remote add origin <link> //bind this remote repository to your local repository. origin" option is the default name for the server on which your remote repository is located.
Example: $ git remote add origin https://github.com/YourUsername/some-small-app.git

	$git push -u origin master // copying your code to a remote repository. our local repo is connected to (origin) and the branch we're pushing, which is master. If we add -u from next time onwards we must only run git push
By using the above two commands, everytime we need to enter the usrname and password registered with that remote repository.  The root of this problem is the HTTPS link you used to connect repositories. Git offers a way out of this inconvenience, 

Approach B]
There's an SSH option that we can use instead of HTTPS. If you set up Git on your computer to work with SSH, then you won't have to enter GitHub credentials every time you push code to GitHub. You'll only need to add a remote origin with this SSH link, like this:

	$ git remote add origin git@github.com:YourUsername/your-app.git

	$git remote -v      //list all the remote repositories that we have

	$git clone <clone>   //Can clone an entire project from the repository
$ git clone git@github.com:YourUsername/your-app.git
If you like to name, the repository that we have cloned 
$ git clone git@github.com:YourUsername/your-app.git name-name

	$git pull          // Running "git pull" is enough to update your local repository.

F.	Branching

	$git branch       //list all git branch
Note: Git will not create a master branch until you commit something.
	$git branch <branch-name> //Creating A new branch
After you've created a new branch to develop a feature, you need to switch to the new branch before you get to work on a feature.
	$git checkout <branch-name>  //For Switching To new branch
	$git merge <branch-name>
	$git checkout -b <branch-name>

If I can Clone a repository. Why do I have to pull something?

Cloning a repository is very different from pulling from a repository. If you clone a remote repository, Git will:
•	Download the entire project into a specified directory; and
•	Create a remote repository called origin and point it to the URL you pass.
The last item simply means that you don't need to run "git remote add origin git@github.com:YourUsername/your-app.git" after cloning a repository. The "clone" command will add a remote origin automatically, and you can simply run "git push" from the repository.
When you run the "pull" command, Git will:
•	Pull changes in the current branch made by other developers; and
•	Synchronize your local repository with the remote repository.
The "pull" command doesn't create a new directory with the project name. Git will only pull updates to make sure that you’re the local repository is up to date.

Why branches are used in GIT
 If you have a stable, working application, you don't want to break it when developing a new feature. Therefore, it's best to have two branches: one branch with a stable app and another one for developing features. Then again, when you complete a feature and it seems to be working, some bug may still be there. And bugs must not appear in a production-ready version. Thus, you'll want to have another branch for testing.
 

In the simplest terms, you'll use branches to store various versions of your project: a stable app, an app for testing, an app for feature development, and so on. Actually, what we've described is just one possible way

whatever I need in a new branch and it won't change the master branch at all. But once I finish developing a feature, how can I move it from that development branch to the master? 
Ans: The steps that needed to be followed:
1.	Create a new branch to develop a new feature using "git branch <branch-name>".
2.	Switch to the new branch from the main branch using "git checkout <branch-name>".
3.	Develop the new feature.

 The answer is simple: you need to use the "merge" command. To merge a secondary branch into the main branch (which can be a master, development, or feature branch), first switch back to the main branch. In our case, we should checkout the master branch:
$git checkout master
*master
	user-profile

The current branch is now changed to master, and we can merge the user-profile branch using the command "merge":
$git merge user-profile
Keep in mind that you're in the main branch and you're merging another branch into the main – not vice versa! Now that the user-profile feature is in the master branch, we don't need the user-profile branch anymore. So let's run the following command:
$ git branch -d user-profile    //Deleting a branch



Use Case1:
If you want to revert back to a older commit in bitbucket.

$git init
$git remote add origin https://kingshuk_nandy@bitbucket.org/kingshuk_nandy/demo.git
$git pull https://kingshuk_nandy@bitbucket.org/kingshuk_nandy/demo.git master
$git log
commit 17f7d477e40f7a25b5862bcbb0b06f8fce67062b (HEAD -> master, origin/master)
Merge: 14b0cfe 64fc34d
Author: Kingshuk Nandy <kingshuk.nandy@infostretch.com>
Date:   Wed Nov 21 14:20:39 2018 +0530

    Merged Other into master

commit 64fc34d22e12fe5d8a0edaa7c6411fe3b9590295 (origin/Other)
Author: Kingshuk Nandy <kingshuk.nandy@infostretch.com>
Date:   Wed Nov 21 08:50:10 2018 +0000

    abc.txt created online with Bitbucket

commit 14b0cfe54c878e35fc9bf65bbf1e2e56825b5ccf
Author: Kingshuk Nandy <kingshuk.nandy@infostretch.com>
Date:   Mon Apr 2 09:53:20 2018 +0000

    Initial commit

$git reset --hard 14b0cfe54c878e35fc9bf65bbf1e2e56825b5ccf


$ git push -f


##Some other Concepts
Git fetch + Git Merge = Git Pull
Git fetch +Git Rebase


Phases:
Fetch--> Merge --> Commit -->Push

##Adding files to git Ignore
......................................
The purpose of gitignore files is to ensure that certain files not tracked by git remain untracked.
Create a .gitignore file all your own:

These steps use vim to update the .gitignore file
Navigate to the folder that contains the files for your project.
If you have not yet created a .git file, run the git commit command.
Create a .gitignore file by running touch .gitignore. The file name ".gitignore" is case sensitive and the name of the file matters. Git will look for a file with that name and will not stage (add) or commit files that is is told to ignore.
Use vim to open the file by running vim .gitignore. This will open a text editor called "Vim" inside your console.
Press the escape key to enter and exit text-entry mode.
Once you have updated your .gitignore files you can save and exit vim by pressing escape, entering :wq, and pressing return/enter
Example:
# Don't track content of these folders
node_modules/
someOtherfoler/


# Compiled source #
###################
*.com
*.class
*.dll
*.exe
*.o
*.so

*.so

# Packages #
############
# it's better to unpack these files and commit the raw source
# git has its own built in compression methods
*.7z
*.dmg
*.gz
*.iso
*.jar
*.rar
*.tar
*.zip







