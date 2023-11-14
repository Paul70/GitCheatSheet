<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [Git Knowledge Base](#git-knowledge-base)
  - [Meaning of HEAD, HEAD~, HEAD~X](#meaning-of-head-head-headx)

<!-- TOC end -->

<!-- TOC --><a name="git-knowledge-base"></a>
# Git Knowledge Base

<!-- TOC --><a name="meaning-of-head-head-heasx"></a>
## Meaning of HEAD, HEAD~, HEAD~X
- HEAD is short for the current commit sha currently checked out.
- HEAD~ or HEAD~1 refers to the commit right before the currently checked out one.
- HEAD~~ or HEAD~2 refers to the the commit even one step eralier as HEAD~ or HEAD~1.
- HEAD~X means the Xth commit before the currently checked out one.
<hr>

## Characterization of Git Rebase and Git Merge

### Git Rebase
Main issue to be aware of in context of Git Rebase is that rebasing one or more commits **changes** the git branch history. 
Main advantage of Git rebase is the maintanence of a straight and linear branch structure (you will see, git merge is the opposite).
rebasing a longer row of commits is tideous and you have to go, commit for commit through the history and solve conflicts.
go throug each commit
open a tool to see the git graph
use vs code or another three window merge tool

### Git Merge
show the characterization which opposites git Rebase
and another 
