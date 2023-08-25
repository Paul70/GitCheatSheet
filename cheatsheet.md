<style>
   body{counter-reset: section}
   h2{counter-reset: sub-section}
   h3{counter-reset: composite}
   h4{counter-reset: detail}

   h2:before{
     counter-increment: section;
     content: counter(section) " ";
   }
   h3:before{
     counter-increment: sub-section;
     content: counter(section) "." counter(sub-section) " ";
   }
   h4:before{
     counter-increment: composite;
     content: counter(section) "." counter(sub-section) "." counter(composite) " ";
   }
   h5:before{
     counter-increment: detail;
     content: counter(section) "." counter(sub-section) "." counter(composite) "." counter(detail) " ";
   }
</style>

<body>
<H1>Git Cheat Sheet</H1>

<H2>Git Workflows</H2>

<H3>Create a local branch and push that branch to the remote repository</H3>

- Step 1: Create a local branch (via git checkout or git branch/switch)
- Step 2: Eventually do already some commits and push the new commits and branch to the remote repository   with set-upstream option
  
        # step 1:
        # git checkout -b <local_feature_branch> 
        [or: 
        # git branch <local_feature_branch> 
        # git switch <local_feature_branch>]

        # step 2:
        # git push -u origin <local_feature_branch>
<hr>

<H3>Rebase a local branch onto origin/main and push origin/main to that new, rebased history</H3>

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
<hr>

<H3>Deleting a remote feature tracking branch</H3>

- Step 1: "Push" delete command to remote and say which remote branch to delete.
- Step 2: In case, a local feature branch is connected with this deleted remote branch, decouple that branch from the now non existing remote branch.
  
        # git push origin --delete <remote_feature_branch>
        # git switch <local_feature_branch>
        # git branch --unset-upstream

<hr>

<H3>Get the newest updates from origin/main for your local feature branch by rebasing your local feature branch commits on top of orgin/main.</H3>

- Step 1: Switch to your local feature branch.
- Step 2: If your local feature branch has a remote tracking branch, update both of them so that they point to the same commit.
- Step 3: Fetch all new commits from the remote repository.
- Step 4: Do an interactive rebase of your local feature branch, i.e. put/squash all your local feature branch commits on top of origin/main.
- Step 5: Now the remote tracking branch and your local feature branch have diverged. Therefore, to bring them together do a push --force (or push --force-with-lease if ohter persons also refer to this remote tracking branch of your local feature branch).
          Optionally, you can do a try run, before pushing for real.

        # step 1 -3:
        # git switch <local_feature_branch>
        # git git push or pull
        [# gitk to verify your git graph]
        # git fetch --all

        # step 4:
        # git rebase --interactive origin/main [to be able to squash commits]

        # step 5;
        [# git push -f --try-run OR git push --force-with-lease --try-run]
        # git push -f OR push --force-with-lease

<hr>

<H3>How to solve merge conflicts happening during a rebase or merge</H3>

It is often a good choice to do a rebase interactively (option --interactive) since you get automatically guided through your rebase process.
In case one just gets command line information to solve conflicts, proceed by 

- Step 1: call your Git mergetool
- Step 2: Solve your conflicts and save everything, when your mergetool editor closes and you return to command line, continue with rebase process

        # step 1:
        # git mergetool [solve your conflicts and whatever has to be done inside your mergetool, save all (CTRL+S) and close mergetool)
        # step 2:
        # git rebase --continue
  
Under section Configuration, there are there is an instruction how to configure VS Code to use it as mergetool and your default Git editor to write commits etc.

<hr>

<H3>Create a local branch tracking an already existing remote branch</H3>

- Step 1: Fetch all remote branch information
- Step 2: List all remote and trackable branches and check if your desired branch is among them 
- Step 3: Create a local feature branch which tracks the desired remote one by giving the local branch the same name without "remotes/origin/".

     # step 1:
     # git fetch --all
     # step 2:
     # git branch -v -a
     # step 3:  
     # git switch
<hr>

## <H3>Merge a remote branch into your local branch after both branches have diverged (mostly after a rebase of the local branch)
Switch to your local branch and do a merge with no commit. Eventually, solve conflicts.

        # git switch <loc_fea_bra>
        # git merge --no-commit <remote_bra_of_loc_fea_bra>
        # git mergetool (optional for solving conflicts)



# <H2>2. Git Configuration 

## <H3> 2.1 VS Code as Git merge tool
To set VS Code as your default git mergetool execute the following commands and you may have a look in your global git config file afterwards.

        # git config --global merge.tool vscode
        # git config --global mergetool.vscode.cmd 'code --wait $MERGED'

Hier muss ich noch festhalten wie ich das mit den drei Spalten bei vs code einstelle, wenn ich conflicts hab

<hr>

# <H3> 2.2 Configure Git for Windows (CR LF) and Unix (LF) line endings
Windows uses two chracters to indicate line endings, CR -Carriage Return - (0x0D (hex), 13 (decimal)) and LF -Line Feed- (0x0A (hex), 10 (decimal)).
Unix based systems only insert a single LF to mark line endings.
However, you need to configure git to adapt the line ending markers if both systems (Windows and Unix) work together on one Git project. This is often the case since build
servers mostly are Unix systems. Thow following command ensures line endings in files you checkout are correct for Windows. For compatibility, line endings are converted to Unix style when you commit files. 

      # git config --global core.autocrlf true

More information about this topic and why the world is as it is can be found under 
  https://stackoverflow.com/questions/1552749/difference-between-cr-lf-lf-and-cr-line-break-types

  https://docs.github.com/en/get-started/getting-started-with-git/configuring-git-to-handle-line-endings

  <hr>

  <H3> Git Command Completion for Windows PowerShell</H3>
  There is a tool/plugin called Git polish or something simialr. How to install and make use of it is depicted here:
   <br>

  https://git-scm.com/book/ms/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Powershell
  
      
### Cached

      #git rm --cached -r .
      # git reset --hard

</body>
        
