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