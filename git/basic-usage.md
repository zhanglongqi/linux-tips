## Show Current Status
```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working tree clean
```

## Tracking New File
```
git add YOUR_FILE_OR_FOLDER
```
If you run your status command again, you can see that your new file is now tracked and staged to be committed:

```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    
```
## Staging Modified Files

Let’s change a file that was already tracked. If you change a previously tracked file called readme.md and then run your git status command again, you get something that looks like this:

```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.md

no changes added to commit (use "git add" and/or "git commit -a")
```
To know more details you can use diff tools to see what have changed of these tracked files:

```bash
$ git diff
diff --git a/readme.md b/readme.md
index 9fd6ba9..dc23ecd 100644
--- a/readme.md
+++ b/readme.md
@@ -1,4 +1,6 @@
 # this is a
 ## second line
-### thrid line
 1. new line
+**some modifications **
+another line
+
(END)
```
From the output of diff tool we can see I deleted one line and add three lines.


