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
```

_Note: if your head is is behind any commits in general be it within main branch or sub branch then you cannot view the commits that are above ur head unless you use `git log --oneline --all`_

**Delete a branch**

```
git branch -d <branch-name>     # if branch is merged
git branch -D <branch-name>     # force delete
```
