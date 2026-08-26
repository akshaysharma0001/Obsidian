What is Git?

Free and open source version control system

What is Version Control system?

. A system that keeps track of our files or projects.
. It allows you to revert selected files to a previous state, revert the entire
project to a previous state, compare changes over time, see who last
modified something so that we can know what might be causing a
problem, or what is the issue, who made it, and when with the details.

Types
Centralized and distributed 

What is GitHub?
GitHub is a web-based hosting service for git repositories.
You can use git without Github, but you cannot use GitHub without Git.

| Git                           |                      GitHub                      |
| ----------------------------- | :----------------------------------------------: |
| Used for Version Control      |        Used for hosting Git repositories         |
| Tracks changes made to a file |                   Cloud based                    |
| Installed locally on computer | Provides a web interface to view file<br>changes |

push: send a change to another repository (may require permission)

pull: grab a change from a repository

#### Basic workflow of Git.

Step 1 - You modify a file from the working
directory.

Step 2 - You add these files to the staging
area.

Step 3 - You perform commit operation that
moves the files from the staging area. After
push operation, it stores the changes
permanently to the Git repository.


##### Blobs
Blob stands for Binary Large Object. Each version of a file is represented by blob. A blob holds the
file data but doesn't contain any metadata about the file. It is a binary file, and in Git database, it is
named as SHA1 hash of that file. In Git, files are not addressed by names. Everything is
content-addressed.

##### Trees
Tree is an object, which represents a directory. It holds blobs as well as other sub-directories. A
tree is a binary file that stores references to blobs and trees which are also named as SHA1 hash
of the tree object.

##### Commits
. Commit holds the current state of the repository. A commit is also named by
SHA1 hash.
. Commit object = a node of the linked list.
. Every commit object has a pointer to the parent commit object.
. From a given commit, you can traverse back by looking at the parent pointer
to view the history of the commit.
. If a commit has multiple parent commits, then that particular commit has been
created by merging two branches.

##### Git Commands
Clone: Bring a repository hosted somewhere like Github into a folder or your local
machine

==Add==: Track your files and changes in Git

==Commit==: Save your files in git

==Push==: Upload your commits to a git repo, like GitHub

==Pull==: Download changes from a remote repository to your local repository.

 git config --global user.name "Akshay"
 git config --global user.email "akshay@gmail"

git init -initialize a folder

git add .

git rm --cached <file to unstage>

git commit -m "Adding demo file "

git remove -v to check if remote repo is configured or not

git remote add origin https://github.com/akshaysharma0001/Demo.git

git push origin master

git config --global credential.helper manager

adding ssh key to github account

ssh-keygen -t rsa -b 4096 -C "akshaysharmas2305@gmail.com"

cat githubssh.pub


