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
git log --online # short one line description
```

**View changes in a commit**

_git-show is a command line utility that is used to view expanded details on Git objects such as blobs, trees, tags, and commits._

```
git show <commit id>
git show <wrt to head pointer> # eg: git show HEAD~2 -> show the commit that is two steps back from head
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
