# Git Cheat Sheet

## Create a local branch and push that branch to the remote repository
Step 1: local branch:   
- using git checkout command:
  
        # git checkout -b <my_new_local_fancy_branch_name>

- using git branch and switch command:
  
        # git branch <my_new_local_fancy_branch_name>
        # git switch <my_new_local_fancy_branch_name>

Step 2: push branch to remote repo (in most cases the remote repo name is "origin"):

        # git push -u <name_of_remote_repo> <my_new_local_fancy_branch_name>
        (# git push -u origin <my_new_local_fancy_branch_name>)

## Fetch the most recent commits from master (or main or trunk) and and do a rebase on your local feature branch <loc_fea_bra>
Step 1: Switch to your <loc_fea_bra> with

        # git switch <loc_fea_bra> 

Step 2: If there is a remote branch of your <loc_fea_bra>, update both branches (git push and pull) so that <loc_fea_bra> and <remotes/origin/loc_fea_bra> point to the same commit.

        # git push or pull or both

Step 3: Fetch all recently added commits from origin:

        # git fetch --all

Step 4: For the sake of couriosity, see what commits have been fetched from remotes/origin/master via

        # git log origin/master ^<loc_fea_bra>

Step 5: Rebase all commits from origin/master into your local branch  <loc_fea_bra>:

        # git rebase origin/master

Step 6: In case of conflicts, resolve them using the git mergetool and commit them on your local branch     
Just close the editor in case you do not want to write a commit. Afterwards, eventually remove generated .orig files which are no longer used.

        # git mergetool (optional command, may go directly in the IDE)
        # git rebase --continue

## Merge a local branch <lob_fea_bra> into master without a commit
Step 1: Switch to master and make everything clean, also update from origin:

        # git switch master
        # git fetch origin
        # git merge --no-commit origin/master

Step 2: Merge local branch <loc_fea_bra> into master without commit (make sure you are on master!):

        # git merge --no-commit <loc_fea_bra>


## Merge a remote branch into your local branch after both branches have diverged (mostly after a rebase of the local branch)
Step 1:

## VS Code as Git merge tool
To set VS Code as your default git mergetool execute the following commands and you may have a look in your git config file afterwards.

        # git config --global merge.tool vscode
        # git config --global mergetool.vscode.cmd 'code --wait $MERGED'