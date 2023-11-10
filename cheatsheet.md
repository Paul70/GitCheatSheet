<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [Git Cheat Sheet](#git-cheat-sheet)
   * [Git Workflows ](#git-workflows)
      + [Create a local branch and push that branch to the remote repository](#create-a-local-branch-and-push-that-branch-to-the-remote-repository)
      + [Rebase a local branch onto origin/main and push origin/main to that new, rebased history](#rebase-a-local-branch-onto-originmain-and-push-originmain-to-that-new-rebased-history)
      + [Deleting a remote feature tracking branch](#deleting-a-remote-feature-tracking-branch)
      + [Get the newest updates from origin/main for your local feature branch by rebasing your local feature branch commits on top of orgin/main](#get-the-newest-updates-from-originmain-for-your-local-feature-branch-by-rebasing-your-local-feature-branch-commits-on-top-of-orginmain)
      + [How to solve merge conflicts happening during a rebase or merge](#how-to-solve-merge-conflicts-happening-during-a-rebase-or-merge)
      + [Create a local branch tracking an already existing remote branch](#create-a-local-branch-tracking-an-already-existing-remote-branch)
      + [Keep a file (folder) locally but do not put it into .gitignore](#keep-a-file-folder-locally-but-do-not-put-it-into-gitignore)
      + [Add Init and Update one or all Submodules listed in your .gitmodules File](#add-init-and-update-one-or-all-submodules-listed-in-your-gitmodules-file)
      + [How to Create a Stash Commit](#how-to-create-a-stash-commit)
   * [Git Configuration ](#git-configuration)
   * [<H3> 2.1 VS Code as Git merge tool](#21-vs-code-as-git-merge-tool)
- [<H3> 2.2 Configure Git for Windows (CR LF) and Unix (LF) line endings](#22-configure-git-for-windows-cr-lf-and-unix-lf-line-endings)
      + [Cached](#cached)
   * [Git Knowledge](#git-knowledge)
      + [Meaning of HEAD, HEAD~, HEAS~X](#meaning-of-head-head-heasx)

<!-- TOC end -->

<!-- TOC --><a name="git-cheat-sheet"></a>
# Git Cheat Sheet

<!-- TOC --><a name="git-workflows"></a>
## Git Workflows 
 
<!-- TOC --><a name="create-a-local-branch-and-push-that-branch-to-the-remote-repository"></a>
### Create a local branch and push that branch to the remote repository

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

<!-- TOC --><a name="rebase-a-local-branch-onto-originmain-and-push-originmain-to-that-new-rebased-history"></a>
### Rebase a local branch onto origin/main and push origin/main to that new, rebased history

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

<!-- TOC --><a name="deleting-a-remote-feature-tracking-branch"></a>
### Deleting a remote feature tracking branch

- Step 1: "Push" delete command to remote and say which remote branch to delete.
- Step 2: In case, a local feature branch is connected with this deleted remote branch, decouple that branch from the now non existing remote branch.
  
        # git push origin --delete <remote_feature_branch>
        # git switch <local_feature_branch>
        # git branch --unset-upstream

<hr>

<!-- TOC --><a name="get-the-newest-updates-from-originmain-for-your-local-feature-branch-by-rebasing-your-local-feature-branch-commits-on-top-of-orginmain"></a>
### Get the newest updates from origin/main for your local feature branch by rebasing your local feature branch commits on top of orgin/main

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

<!-- TOC --><a name="how-to-solve-merge-conflicts-happening-during-a-rebase-or-merge"></a>
### How to solve merge conflicts happening during a rebase or merge

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

<!-- TOC --><a name="create-a-local-branch-tracking-an-already-existing-remote-branch"></a>
### Create a local branch tracking an already existing remote branch

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

<!-- TOC --><a name="keep-a-file-folder-locally-but-do-not-put-it-into-gitignore"></a>
### Keep a file (folder) locally but do not put it into .gitignore
There are several strategies:

https://stackoverflow.com/questions/936249/how-to-stop-tracking-and-ignore-changes-to-a-file-in-git

https://luisdalmolin.dev/blog/ignoring-files-in-git-without-gitignore/#:~:text=To%20ignore%20untracked%20files%2C%20you,tracking%20any%20(untracked)%20file.

- Strategy 1: Keep the local file (or folder) but delete it for anyone else who pulls, i.e. do not publish it
  
      # git rm --cached <path_to_file/file_name>
      [ #or git rm -r --cached <path_to_folder/folder_name>]
   
- Strategy 2:
- Strategy 3: 

<hr>

<!-- TOC --><a name="add-init-and-update-one-or-all-submodules-listed-in-your-gitmodules-file"></a>
### Add Init and Update one or all Submodules listed in your .gitmodules File

Updating a submodule for the first time (after one has added a submodule) requires an init step. In case, one has cloned a git project with "--recurse-submodules", this 
init step is included and one can directly update the submodules:
Das hier ist noch nicht vollständig, da muss noch so ein pull --recurese-submodules und so rein

- Initially update all your project's submodules recursively, i.e. alos update all nested submodules (not necessary after "git clone --recurse-submodules")

      # git submodule update --init --remote --recursive

- Update all your (already initialized) submodules
  
      # git submodule update --recursive


<!-- TOC --><a name="how-to-create-a-stash-commit"></a>
### How to Create a Stash Commit

Git stash command offers the 


<!-- TOC --><a name="git-configuration"></a>
## Git Configuration 

<!-- TOC --><a name="21-vs-code-as-git-merge-tool"></a>
## <H3> 2.1 VS Code as Git merge tool
To set VS Code as your default git mergetool execute the following commands and you may have a look in your global git config file afterwards.

        # git config --global merge.tool vscode
        # git config --global mergetool.vscode.cmd 'code --wait $MERGED'

Now, in case of conflicts, you can just call VS Cosde to solve all conflicts

        # git mergetool

In the lower right corner click on "Mergetool" to get into the three column merge tool view. If you do not get the middle column "Base" at startup, click on the three dots in the most 
upper right corner and select "Show Base"

Now you have three columns and a bottom window:
- the most left column shows you code which is coming from a remote commit you have recently fetched and which is conflicts with one of your commits. See also the commit hash and look it up with gitk
  or another graph tool.
- the middle column called "Base" shows the base or parent state without changes neiter from the remote incoming commit nor from your individula, local commits
- the right column represents the content of your commits you did on your local machine and you want to merge/rebase/pull

<hr>

<!-- TOC --><a name="22-configure-git-for-windows-cr-lf-and-unix-lf-line-endings"></a>
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
  
      
<!-- TOC --><a name="cached"></a>
### Cached

      #git rm --cached -r .
      # git reset --hard

<!-- TOC --><a name="git-knowledge"></a>
## Git Knowledge

<!-- TOC --><a name="meaning-of-head-head-heasx"></a>
### Meaning of HEAD, HEAD~, HEAS~X
- HEAD is short for the current commit sha currently checked out.
- HEAD~ or HEAD~1 refers to the commit right before the currently checked out one.
- HEAD~~ or HEAD~2 refers to the the commit even one step eralier as HEAD~ or HEAD~1.
- HEAD~X means the Xth commit before the currently checked out one. 


        
