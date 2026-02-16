Git is a version control system, which keeps track of changes made to files over time. Features include being able to revert back to previous states, compare changes made between two versions, collaborate with other people, etc.

There are 3 main components to a version control system:

- The repository/repo is the location of the main version of the source code. This can be local or remote(for ex. Github).
- The staging area represents the changes from the working directory.
- The working directory is a copy of the repo, where you make changes before saving them.

Originally our files are in the working directory. To add them to the changeset in the staging area, use git add. To then save them to the local repo, use git commit. Finally, to keep local and remote repo synced, use git push/git pull.

To initialize a local repo, use git init within the project directory. This creates a .git directory which will track files. We can use git status to check the status of our files (untracked, staged, committed, etc.). To explore our commit history, we can use git log.

To compare modifications between the same file at two different times, we can use git diff. To runtrack a file, we can do git rm \[filename]. To rename a file, we can do git mv \[oldname] \[newname]. If there are many files in the directory which are not tracked and not part of the project, we can run git clean.

## Branches

Let's say you want to test out a new development feature, but you want to make sure it works well without affecting the main development branch. One way to do so is to create your own branch of the code. By default, we are on main branch. To create a branch, we can do git branch \[branchname]. It will point to the same commit as main.

When we do git checkout \[branchname], we move to the new branch and anything we do will be in this branch. Now, we could do a new commit. This commit wouldn't appear in main branch.

Let's say the committed feature works fine. We can use git merge \[branchname] to merge the feature back into main.

Let's say that in fact, someone else made a separate commit on the main branch while we were doing stuff in our own branch.

![[Pasted image 20260215213236.png]]

This might be troublesome, since the branches have diverged. It turns out Git's merge capabilities are pretty powerful. As long as the changes made in commit D and E do not intersect with each other, Git will incorporate both of them, like this:

![[Pasted image 20260215213341.png]]

If there are intersecting changes in the commits made in different branches, then merging causes a merge conflict. Git will ask the user to resolve it themselves. You will have to open the file which had conflicts, and there will be annotations of where the conflicts are. The developer should then edit it manually, remove the annotations, then add+commit the file normally.

Let's say you pushed out a feature which has mistakes and does not work properly, and you wish to undo it. There are 3 ways to do so:

- git revert: reverts the changes made in a commit
- git reset: resets the HEAD pointer (and the main branch) to the specific commit id. This command can be dangerous sometimes in that it will truly reset all the files you worked on, losing all the previous work.
- git checkout: we can checkout what the code looked like at a previous commit, and then checkout a new branch at that time
