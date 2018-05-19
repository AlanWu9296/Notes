# Setup

1. `$ ssh-keygen -t rsa -C "your_email@youremail.com"` generate ssh key and set the key in the **github** account setting
2. `$ ssh -T git@github.com` to test whether it is set successfully
3. `$ git config --global user.name "your name"`                                                                          `$ git config --global user.email "your_email@youremail.com"`
4. `$ git remote add origin git@github.com:yourName/yourRepo.git` set remote address
5. `git clone username@host:/path/to/repository` to clone remote server or `git clone /path/to/repository` to clone local server 

# Work Flow

1. `git add <filename>` or `git add .`to add all files to Index(storage)
2. `git commit -m` to commit the changes to HEAD
3. `git push origin <branch name>` push <branch> to the remote server

# Branch

1. `git branch` to check all the existing branches
2. `git branch <branch name>` or `git checkout -b <branch name>` to create a new branch
3. `git checkout <branch name>` to goto <branch name>
4. `git branch -d <branch name>` to delete <branch name>
5. `git pull` to update local as the remote
6. `git merge <branch name>` merge <branch name> into current branch
7. `git diff <source> <target>` compare two branches difference

