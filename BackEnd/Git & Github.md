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

# Revert

## Temporarily switch to a different commit

```bash
# This will detach your HEAD, that is, leave you with no branch checked out:
git checkout 0d1d7fc32
```

## Hard delete unpublished commits

```bash
# This will destroy any local modifications.
# Don't do it if you have uncommitted work you want to keep.
git reset --hard 0d1d7fc32

# Alternatively, if there's work to keep:
git stash
git reset --hard 0d1d7fc32
git stash pop
# This saves the modifications, then reapplies that patch after resetting.
# You could get merge conflicts, if you've modified things which were
# changed since the commit you reset to.
```

## Undo published commits with new commits

```bash
# This will create three separate revert commits:
git revert a867b4af 25eee4ca 0766c053

# It also takes ranges. This will revert the last two commits:
git revert HEAD~2..HEAD

#Similarly, you can revert a range of commits using commit hashes:
git revert a867b4af..0766c053 

# Reverting a merge commit
git revert -m 1 <merge_commit_sha>

# To get just one, you could use `rebase -i` to squash them afterwards
# Or, you could do it manually (be sure to do this at top level of the repo)
# get your index and work tree into the desired state, without changing HEAD:
git checkout 0d1d7fc32 .

# Then commit. Be sure and write a good message describing what you just did
git commit
```

## Conflict

## keep the local version

```bash
git stash
git pull
git stash pop
```

## keep remote version

```bash
git reset --hard
git pull
```

