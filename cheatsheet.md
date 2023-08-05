<H1>Git Cheat Sheet

#<H2>1. Git Workflows

##<H3>1.1 Create a local branch and push that branch to the remote repository
- Step 1: Create a local branch (via git checkout or git branch/switch)
- Step 2: Eventually do already some commits and push the new commits and branch to the remote repository with set-upstream option
  
        # step 1:
        # git checkout -b <local_feature_branch> 
        [or: 
        # git branch <local_feature_branch> 
        # git switch <local_feature_branch>]

        # step 2:
        # git push -u origin <local_feature_branch>
<hr>

## <H3>1.2 Rebase a local branch onto origin/main and push origin/main to that new, rebased history
- Step 1: Checkout your local branch you want to rebase and get updates, especially for remote origin/main by doing a fetch all. Eventually have a look on the commit graph (e.g. with gitk).
- Step 2: Do a interactive rebase onto origin/master. In case of squashing commits, remember that you must pick the first commit. Also give a meaningful squash commit description. If conflicts occur, resolve them.
- Step 3: Switch to your local main and simply fast-forward it to origin/main. That should be possible since you do not work directly on your local master branch.
- Step 4: Let your local main branch point to the rebased commit on top of your history by fast-forward merging your local branch into your local master. Note, up to this step, you did not change anything on origin/main.
- Step 5: First check, if you can update origin/main pushing dryly and then do a real push.
- Step 6: If you are sure, everything is good and you do not longer need the remote branch of your local feature branch (the one you rebased onto origin/main), you can switch to your local feaure branch and do a foreced push to update it. You need to push forcefully since your local feature branch and its associated tracking branch have diverged due to the rebase done before.
  
        # step 1:
        # git switch <local_feature_branch>
        # git fetch --all
        [# gitk]

        # step 2:
        # git rebase --interactive origin/main [to be able to squash commits]

        # step 3:
        # git switch main
        # git merge --ff-only 

        # step 4:
        # git merge --ff-only <local_feature_branch>

        # stpe 5:
        # git push --dry-run [if everything is ok continue with next command]
        # git push

        # step 6:
        # git switch <local_feature_branch>
        # git push -f 

## <H3>1.3 Fetch the most recent commits from master (or main or trunk) and and do a rebase on your local feature branch <loc_fea_bra>
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
<hr>

## <H3>Merge a local branch <lob_fea_bra> into master without a commit
Step 1: Switch to master and make everything clean, also update from origin:

        # git switch master
        # git fetch origin
        # git merge --no-commit origin/master

Step 2: Merge local branch <loc_fea_bra> into master without commit (make sure you are on master!):

        # git merge --no-commit <loc_fea_bra>
<hr>

## <H3>Merge a remote branch into your local branch after both branches have diverged (mostly after a rebase of the local branch)
Switch to your local branch and do a merge with no commit. Eventually, solve conflicts.

        # git switch <loc_fea_bra>
        # git merge --no-commit <remote_bra_of_loc_fea_bra>
        # git mergetool (optional for solving conflicts)

### Fast foreward a branch to another
Switch to the branch you want to fast-foreward and then do a fast foreward merge (merge -ff) by specyfing the <destiny_bra> name to which you want to fast forward your <loc_fea_bra_to_ff>:

        # git switch <loc_fea_bra_to_ff>
        # git merge --ff --no-commit <destiny_bra>

### Create a local branch tracking an already existing remote branch (which is not trunk!)
Steps to reach the goal:
  1. Fetch all remote branch information
  2. List all remote and trackable branches and check if your desired branch is among them 
  3. Create a local branch which tracks the desired remote one (e.g. remote branch is called "remotes/origin/remote/feature/branch") by giving the local branch the same name without "remotes/origin/".

  $ git fetch --all
  $ git branch -v -a 
  $ git switch <remote/feature/branch>


## 2. Git Configuration 

### VS Code as Git merge tool
To set VS Code as your default git mergetool execute the following commands and you may have a look in your global git config file afterwards.

        # git config --global merge.tool vscode
        # git config --global mergetool.vscode.cmd 'code --wait $MERGED'

### Configure Git for Windows (CR LF) and Unix (LF) line endings
Windows uses two chracters to indicate line endings, CR -Carriage Return - (0x0D (hex), 13 (decimal)) and LF -Line Feed- (0x0A (hex), 10 (decimal)).
Unix based systems only insert a single LF to mark line endings.
However, you need to configure git to adapt the line ending markers if both systems (Windows and Unix) work together on one Git project. This is often the case since build
servers mostly are Unix systems. Thow following command ensures line endings in files you checkout are correct for Windows. For compatibility, line endings are converted to Unix style when you commit files. 

      # git config --global core.autocrlf true

More information about this topic and why the world is as it is can be found under 
  https://stackoverflow.com/questions/1552749/difference-between-cr-lf-lf-and-cr-line-break-types

  https://docs.github.com/en/get-started/getting-started-with-git/configuring-git-to-handle-line-endings

  
      
### Cached

      #git rm --cached -r .
      # git reset --hard
        
