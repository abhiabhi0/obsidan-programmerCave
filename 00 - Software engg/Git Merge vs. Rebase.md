![[{00980DC9-0061-401A-AB8F-0AFE7B1064D7}.png]]

Git Merge
This creates a new commit G’ in the main branch. G’ ties the histories of both main and feature branches. 
Git merge is **non-destructive**. Neither the main nor the feature branch is changed.

Git Rebase
Git rebase moves the feature branch histories to the head of the main branch. It creates new commits E’, F’, and G’ for each commit in the feature branch. 
The benefit of rebase is that it has linear commit history. 
Rebase can be dangerous if “the golden rule of git rebase” is not followed. 

The Golden Rule of Git Rebase Never use it on public branches!