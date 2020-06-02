## Git Commands Cheatsheet

Add Local upstream

git remote add <NAME> <PATH>
cd remote-local/
git init --bare
git remote add bak remote-local/.git

Add Upstream

git remote add <remote_name> https://github.com/OWNER/REPOSITORY.git
git remote -v # To check
git remote show <remote_name> # Common Remote Names - origin, upstream
git remote set-url <remote_name> https://github.com/OWNER/REPOSITORY.git
git remote remove <remote_name>


Update local and your fork (Master Branch Only? )

git fetch upstream
git checkout master
git merge upstream/master
git push -f origin master

Unstage/Delete Changes

git checkout -- <file>  #to discard changes in the working directory
git reset HEAD <file>  #to unstage

Git Clean 

git clean -n (see)
git clean -f (delete)

Git Credentials Help

git config --global credential.helper store #Set Credentials on next push
git config --global credential.helper 'cache --timeout=3600'  # 1 Hour Timeout (in sec)
git config --global --unset credential.helper

Fresh copy for a specific branch

git checkout master
git reset --hard origin/master

Fresh copy whole codebase

git reset --hard HEAD^
git clean -fd
git fetch
git rebase origin/master

Squash commits/editing commit message

git rebase -i HEAD~n      ##Replace ‘n’ with the number of commits you to edit/squash
git push origin branch-name --force

Edit Last Commit Message (If No changes after Commit)

git commit --amend

Add more Changes to last commit (If changes after Commit)

git commit --amend


Stashing and Cleaning

git stash 
git stash list
git stash apply
git stash apply stash@{2}

Show Branches (and current branch) with commits in CLI

git log --oneline --decorate --graph --all (All Branches)
git log --oneline --decorate

Push Branch to Origin Repo if not present already

git push --set-upstream origin <branch_name> # origin -> <remote_name>

Delete Branch from Origin Repo

git push -d <remote_name> <branch_name> # origin -> <remote_name>
git branch -d <branch_name> # After Checking Out

Push only up to a specific Commit
If say there are 5 commits A(oldest)>B>C>D>E(newest) and only A, B, C are to be pushed, we use commit C’s hash is <commit hash>

git push <remote> <commit hash>:<branch> 
git push origin serverfix:awesomebranch

Push Specific Branches Only

git push <remote_name> <branch_name>
git push origin serverfix:awesomebranch


Use a remote tracker to set up an editable local branch
After fetching it’s only a tracker. 

git checkout -b <branch> <remote>/<branch>
git checkout --track <remote>/<branch>
git branch -vv


Checking tracker info

   git fetch --all 
   git branch -vv

For a local branch changing the upstream tracker/ setting the tracker to a branch just fetched

   git branch -u origin/serverfix
   git branch --set-upstream-to origin/serverfix




Simple Rebase

   git checkout experiment
   git rebase master 	# This applies the current branch/experiment to master
   git rebase <basebranch> <topicbranch> # Does the same work as above two
   git checkout master	
   git merge experiment	# This fast forward merges the branch


Complex Rebase
“Take the client branch, figure out the patches since it diverged from the server branch, and replay these patches in the client branch as if it was based directly off the master branch instead.” 


   git rebase --onto master server client
   git checkout master
   git merge client		# This is again a fast forward merge



Going To a Previous Commit

git checkout <sha-1 of that commit>

Going To a Previous Commit and Creating a New Branch From That Commit. Then pushing the branch to remote.  

   git checkout <sha-1 of that commit>
   git checkout -b <branch-name>
   git push --set-upstream <remote-name> <branch-name>

Setting current branch to track a remote branch

   git branch --set-upstream-to=origin/master
   git branch -vv


# ToDo

1. Setup git mergetool to use vimdiff and explore merge conflict resolving
2. Explore ./gitconfig

