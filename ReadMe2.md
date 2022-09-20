# My Git CheatSheet

**Display the status of the working directory and the staging area**

```
git status
git status -s # s flag here is short to show status info in short oneline
```

**Move the files from working directory to staging area**

```
git add <file name>
git add .   # add all files
```

**Stop git from tracking a file: remove from index without touching the working directory copy**

```
git rm --cached <file name>
```

**Commit changes**

```
git commit -m "Meaningful Message preferably in present tense representing logical unit of work"
git commit # opens a text editor to give a type message
git commit -am <message> # directly commit files without staging, do this only when u r confident of the changes
```

_Note: Even after commit, the files are present in staging area to track the changes of the present file against the changes in the working directory_

**Ignore files**

To Ignore git tracking the changing of certain unncessary file add them in <i>.gitignore</i> file. Make sure the files added are not already tracked by git. Else remove those files from the staging area and then add.

**See Difference in changes in the files**

```
git diff    # changes in working dir
git diff --staged # changes in files of staging area
git difftool    # too see via a diffing tool for files in working dir
git difftool --staged # for files in staging area
```

**View history of commits**

```
git log
git log --online # short one line description : displays from where head is
git log --online --all # displays all commits regardless of where head is
git log --oneline --all --graph # visualise branches
git log <commit id> -<number> # display last n commits from that commit id
```

**View changes in a commit**

_git-show is a command line utility that is used to view expanded details on Git objects such as blobs, trees, tags, and commits._

```
git show <commit id>
git show <wrt to head pointer> # eg: git show HEAD~2 -> show the commit that is two steps back from head
```

**View list of files changed in a commit**

```
git show <commit id> --name-status
```

**List files in a commit**

```
git ls-tree HEAD / <commit id>
```

**Remove local changes to previos version of commit**

```
git restore . | git restore <file name>
```

_Note: The above command only works for tracked files (i.e. in staging area) because only they have a prev version. New files aren't tracked yet and can be maually deleted or using the following command_

```
git clean -fd  # force remove directory
rm <file name> # but can only remove one file
# or just manually delete the file(s), git gives no fks about to the untracked files.
```

**Unstage a file**

```
git restore --staged <file name>
```

_Note: `git rm --cached file`: removes the copy of the file from the index / staging-area, without touching the working tree copy. The proposed next commit now lacks the file. If the current commit has the file, and you do in fact make a next commit at this point, the difference between the previous commit and the new commit is that the file is gone. `git restore --staged` file: Git copies the file from the `HEAD` commit into the index, without touching the working tree copy. The index copy and the `HEAD` copy now match, whether or not they matched before i.e. simply it unstages the files and restores file to the version of file in the prev commit. If the current commit lacks the file, this has the effect of removing the file from the index. So in this case it does the same thing as `git rm --cached`._

**Restore files to any previous version**

```
git restore --source=HEAD~1 <file name>
```

**Set alias for command**

```
git config --global alias.unstage "restore --stage"
```

**Navigate between branches**

```
git checkout <commit id> # moves head to that commit
```

_Note: During checkout do not making any commit. Else the commit will become unreachable and useless called Dead Commit_

**To find Which commit in your project histroy introduced bug**

```
# works like binary search to find the buggy commit

git bisect start            # start bisect operation
git bisect bad <commit id>  # tell bisect about the latest bad commit
git bisect good <commit id> # tell bisect about the latest good commit

# binary search start - head detaches to the mid of the good and bad

git bisect bad  # if commit is found to be buggy
git bisect good # if commit is found to be fine
git reset       # so that head is attached back to the master after finding the buggy commit
```

**To view contributors of the repo**

```
git shortlog -s # s flag to show authors name only
```

**View History of log file**

```
git log --oneline <file name>
git log --oneline --patch <file name>   # to view the actual changes in the file in each commit
```

**Tags**

_Tags are ref's that point to specific points in Git history. Tagging is generally used to capture a point in history that is used for a marked version release (i.e. v1. 0.1)_

```
git tag     # view list of all tags
git tag
```

- **LightWeight Tag** : A lightweight tag is very much like a branch that doesn’t change — it’s just a pointer to a specific commit.

```
git tag v1.0
git tag -d v1.0 # delete tag
```

- **Annotated Tags** : Annotated tags, however, are stored as full objects in the Git database that shows the tagger information, the date the commit was tagged, and the annotation message before showing the commit information.

```
git tag -a v1.0 -m "My version  v1.0"
git tag -d v1.0 # delete tag
```

_Note: Prefer Annoted Tags over Lightweight tags_

We can Referencing with tags

```
git show v1.0
```

### Branching

**Create a branch**

```
git branch <branch name>
```

**View all branches**

```
git branch
```

**Switch to another branch**

```
git switch <branch-name>
git switch -C <branch-name>     # create and switch to the branch
```

_Note: if your head is is behind any commits in general be it within main branch or sub branch then you cannot view the commits that are above ur head unless you use `git log --oneline --all`_

**Delete a branch**

```
git branch -d <branch-name>     # if branch is merged
git branch -D <branch-name>     # force delete
```

**Stashing**

_Note: When do i use stashing? : When you have two same file in two commit/branches and you want to checkout your head to that commit/branch, then git tries to overwrite the files with the content in the "checkouting" commit/latest commit of branch. In that case the local changes in your current commit files is lost. In that case, to not let git overwrite your files, you stash. If one understands how checkout works then understanding stash is simple. Checkout updates the files in the working directory to match the version stored in that branch/commit, and it tells Git to record all new commits on that branch/commit_

```
git stash push -m "<message>"
```

**View stash list**

```
git stash list
```

**show files changes in stash**

```
git stash show <stash number> # git stash show 1 => refer stash number from stash list
```

**Bring back files to wokring directory**

```
git stash apply 1
```

**Delete a stash**

```
git stash drop 1
```

**Delete all files in stash**

```
git stash clear
```

### Merging

- Fast forwarding: When two branch doesnt diverge [Fast-forwarding illustration](https://bitbucket.org/blog/wp-content/uploads/2018/07/what-is-a-fast-forward.gif)
- 3-way merging: when two brach diverge (in this case we may resolve conflicts between two versions) [3-way merging illustration](https://www.w3docs.com/uploads/media/default/0001/03/492bbdd4d1c256ad4b07c9a042fd18fca353b1cc.png)

**Fast forwarding**

```
git merge <branch name> # from the parent branch, applicable for both type of merge
```

```
git merge --no-ff <branch name>     # no fast forward even when its possible
```

_Note: `No fast forward (--no-ff)` will merge branch with a new commit. So if we want to revert the feature brach then only one revert is needed while ff need n reverting where n is the number of commit in that sub branch. Also noff is the true representation of history_

**View merge and no-merged branches**

```
git branch --merged
git branch --no-merged
```

**Aborting a merge**

_During merge conflicts we can abort that merge for time being_

```
git merge --abort
```

**Undo commits** (Applies to even undoing merge as merging is also a commit) <br/>

<u>Three reset options </u> <br/>
Hard: restores everything to the orginal state of that commit <br/>
Mixed: will move every files ahead of pointing commmit into unstaged area _(Default)_<br/>
Soft: will move every files (expcept unstaged files) ahead of pointing commmit into staged area <br/>

```
git reset --hard <commit id>
git reset --mixed <commit id> (or) git reset <comit id>
git reset --soft <commit id>
```

_Note: Undoing commits will rewrite history. Use this only in your local repository but not on the remote repository_

**Reverting merge/commit**

if you want to undo commits in remote repo as well then create a new commit that undoes the previos commit

```
git revert -m 1 HEAD
```

**Squash merging**

We squash the commit(s) in the sub branch in to one sigle commit and add it on top of main branch. This is done to make main branch look linear as if no branch was created. Only single commit that does the job instead of many commit and merges polluting the branch. <br/>

[Squash merging illustration](https://user-images.githubusercontent.com/67074796/191254285-40e048ae-9fab-4657-8686-581808d5c34f.jpg)

```
git merge --squash <branch-name>
```

_Note: This is not a merge though it is called merging as we are not actually merging the branch but only squashing the commits and appending it on top of the main branch. Make sure to delete the branch using `git branch -D <branch-name>`. `-D` because we are force deleting as the branch is not acutally merged._

**Rebasing**

Changing the base of the sub branch to the lastest commit in the parent branch. This is lead to a linear history.

[Rebasing illustration](https://cms-assets.tutsplus.com/cdn-cgi/image/width=600/uploads/users/585/posts/23191/image/rebase-2.png)

```
# from the subranch
git rebase master       # rebase the subranch to the latest commit is master
```

```
git rebase --continue   # move on to next conflict if ther is any
git rebase --skip       # skip a commit
git rebase --abort      # abort rebasing
```

```
# After rebasing head pointer can be moved to the latest commit by fast forwarding
git merge <branch-name>     # This will fast forward as the history is now linear
```

_Note: Git accomplishes this by creating new commits and applying them to the specified base. It's very important to understand that even though the branch looks the same, it's composed of entirely new commits. This operation will rewrite history, so be careful!_

**Cherry Picking a commit**

Enables arbitrary Git commits to be picked by reference and appended to the current working HEAD.
[Cherry picking illustration](https://www.junosnotes.com/wp-content/uploads/2021/07/how-do-i-cherry-pick-a-commit-in-Git.png)

```
git cherry-pick <commit id>     # this will add the commit on the top of the commit that head is pointing to.
```

_Note: May lead to conflicts, resolve conflicts if any in the process of cherry picking_

**Picking files from another branch**

```
git restore --source=<branch name> <filename>
```

### Collaboration

**Clone a repo**<br/>

Create a copy of that repo at in a new directory/machine.

```
git clone <github repo url> <your-project-name>
```

**Get details of remote repo**

```
git remote -v
```

**Fetch contents from remote repo** <br/>
used to download contents from a remote repository.

```
git fetch
```

_Note: use `git branch -vv` to see how our local and remote branch are diverging. use `git merge` to merge the remote branch with local branch_

**Git pull**

> **pull = fetch + Merge**

Used to fetch and download content from a remote repository and immediately update (perform merge operation) the local repository to match that content

```
git pull [type of merge]
git pull            # performs fast forward or 3-way merging depending on linearity of branches
git pull --rebase   # fetch + rebasing
```

_Note: while rebasing local commits are added on top of orgin (remote) branch_

**Git push** <br/>

Used to upload local repository content to a remote repository.

```
git push
```

_Note: Before commiting make sure `origin/master` of local repo and `master` of remote remo are pointing to same commit. Only then make a push, else git will reject as the local and remote branch have been divereged._

**Sharing Tags**<br/>

Our tags aren't shared by git in general and needs to be pushed explictly.

```
git push origin <tag-name>                          # push tag
git push origin --delete <tag-name></tag-name>      # delete tag
```

**Share Branches** <br/>

Like Tags, we need to explicitly share branches <br/>

```
git push -u origin <branch-name>
git push --delete origin <branch-name>
```

_Note: Even though you delete branch in remote is note deleted and you need to delete it explicitly using `git branch -d <branch-name>`_

**Prune branches** <br/>

If there are any branches that has been deleted in remote, then it is not updated in local even after fetch, so we use:

```
git remote prune origin
```

**Keeping forked repository up to date**

```
git remote  # show all remote repo
git reomte -v  # show all remote repo with verbose (extra info)
```

Add another reference to base repo and push the changes to the forked repo. [illustration](https://user-images.githubusercontent.com/67074796/191335281-c731d732-41ba-4f01-ab6f-a820e57e5a50.jpg). <br/>

```
git remote add <any name of base repo> <base repo url>      # add a remote repo url
git rename <old name> <new name>    # rename base repo alias
git remote rm <base repo name>      # delete a remote refernce
```

```
git fetch base  # fetch changes from the base repo
git merge base/master
git push    # push the base changes to our forked repo
```

### Rewriting History

**Dont rewrite public history!** <br/>
if no one will get affected by your rewriting public history then go ahead force pushing. Now all the commits that have been updated after the your last recent fetch will get lost.

```
git push -f     # when github rejects due to conflicts with local repo and remote repo
```

_Note: However, rewriting history in local is a good practice, so that you can tidy up your histry and only push clean history to the remote repo._

**Undoing commits**

```
git reset --[soft/hard/mixed] <commit id>
```

> Soft: Removes the commit only, all commit files are in staging area and unstaged files are unstaged <br/>
> Mixed: All the commit and unstaged files become unstaged <br/>
> Hard: All files are discarded - A state just after you make that commit

**Revert a commit** <br/>

If you made a bad commit and if its already pushed to your remote, Then don't `git reset --hard head~1`, this will rewrite public history and that could be dangerous! Instead make a new commit that undoes the changes of last bad commits.

```
git revert <parent commit id of bad commit> # git revert head~1
git revert <parent commit id>..<bad commit id>  # revert commit in range
e.g.: git revert head~3..head (or) git revert head~3.. # will automatically remove till head
```

_Note: Git revert in range will cause multiple reverts commits and makes the history ugly, instead use `--no-commit` to make one combined commit on behalf of all with our custom commit_

```
git revert head~3..head --no-commit
```

```
git revert --continue   # move to next revert
git revert --abort      # cancel the revert
```

**Recover a lost commit** <br/>

The reflog is an ordered list of the commits that HEAD has pointed to. Its purely local.

```
git reflog
```

Fing the lost commit and then do `git reset --hard <commit id from reflog>`.

**Amend a commit** <br/>

Amending a commit is a way to modify the most recent commit you have made in your current branch. This can be helpful if you need to edit the commit message or if you forgot to include changes in the commit. you can also do the same thing using `git reset --mixed head~1`. If you want to delete a file in prev commit then uset `git reset`. [Rebasing to ammend commits illustration.](https://user-images.githubusercontent.com/67074796/191364005-b4cce155-067c-4ee1-8e44-00fe696c2b01.jpg). Here 'B' is the commit to be ammended. But new duplicate commits are also created for C and D as commits are immutable in git.

```
git commit --amend -m "<message>"
```

**Amend Earlier commit** <br/>

we use rebasing to achieve it

```
git rebase -i <commit id of parent of amending commit>  # in the interactive mode change pick to edit, to ammend those commit(s).
git commit --amend      # now amend the commit
git rebase --continue   # to perform rebasing
git rebase --abort      # if you screwed things up
```

**Drop a commit** <br/>

Again we use rebasing for prev commits that is not latest

```
git rebase -i <commit id of parent of amending commit>      # in the interactive mode edit pick to drop or just delete those commit(s) lines
```

**Rewording a commit** <br/>

If we the commit is the most recent one, then use `git commit --amend`. else again we use rebasing in interactive mode with reword key

```
git rebase -i <commit id of parent of amending commit>      # in the interactive mode edit pick to reword, to reword those commit(s)
```

**Reordering commit**<br/>

Again we use rebasing. In the rebase interactive mode, just move the commit to the desired position.

**Squasing commits** <br/>

Again we use rebasing. In the rebase interactive mode, just change pick to `squash` of `fixup`. This will squash the commits. <br/>
_Note: only apply squash/fixup to commit that need to be squash with the other commit. Not require for commit to which others will be squashed_ <br/>
_Only difference between squash and fixuup is squash will combine commit message of all the commits that will be squashed, which again be edited in editor while fixup takes the commit of the commit to which other commits will be squashed, and applies that message to all._

```
git rebase -i <commit id of parent of amending commit>  # in the interactive mode edit pick to squash/fixup, to squash those commit(s)
```

- **_Note: In interactive rebasing, you can squash, fixup, reoder, reword, pick etc at same time_**

**Split commits**

Again we use rebasing. Nn the interactive mode change pick to edit. We now reset one step backwares using `git reset --mixed head~1`. Then do the approriate commits.

```
git rebase -i <commit id of parent of amending commit>  # in the interactive mode change pick to edit, to ammends those commit(s).
git reset head^         # move 1 commit back head~1 = head^
(git add <desired filename>, git commit -m) * n
git rebase --continue   # to perform rebasing
git rebase --abort      # if you screwed things up
```
