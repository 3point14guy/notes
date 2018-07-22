# Exercise - Your first repository
In this exercise we will create our first repository. We will touch on common commands and point out some quirks like how git works with empty directories.

1. Create the following directory structure on your desktop
```
git_exercise
|
│   index.html  
|
└───assets
    │
    └───images
        |
        javascripts
        |
        stylesheets
        |
```
2. Open your terminal or git bash for you windows folks and change directories to the root directory of your project structure
3. Run `git init` to initilize your repository
4. Run `git status` to see that status of your repository. You should see output similar to this.
```
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

  index.html

nothing added to commit but untracked files present (use "git add" to track)
```
5. Notice that it shows index.html as an untracked file. Where is your assets directory and its sub directories? Git does not keep track of empty directories
6. Remedey the above with by adding a .keep file to each directory under assets or adding relevent files to those directories if you have them
7. Run `git status` to see that status of your repository. You should now see your assets directory being tracked
```
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

  assets/
  index.html

nothing added to commit but untracked files present (use "git add" to track)
```
8. Run `git add .` to add our files to our staging area to be commited. `git add` is the command to stage files for a commit. `.` is the path to what we want to add. `.` is everything in the current directory.
9. Run `git status` to see that status of your repository. You no longer have unstracked files. You have a list of files to be commited.
```
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

  new file:   assets/images/.keep
  new file:   assets/javascripts/.keep
  new file:   assets/stylesheets/.keep
  new file:   index.html

```
10. Lets create our first commit. Run `git commit -m "Initial commit"`. This command will take all the files to be commited and create a point in history related to these changes. A commit should ecompass all the changes related to a certain task or logical set of changes
11. Run `git log` to see you commit history
```
commit bddf8d41d0f8f6d0c9ebdb0677e2913b55da9311
Author: Eric Schwartz <eric@techtalentsouth.com>
Date:   Wed Feb 15 16:36:20 2017 -0500

    Initial commit
```
12. Open index.html and add a basic html structure to the page
13. Run `git status` to show that you have modified the index.html file since your last commit
```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

  modified:   index.html

no changes added to commit (use "git add" and/or "git commit -a")
```
14. Run `git diff` to see what has changed in the file that has been modified. Lines added to the file will have a + next to them. Removed lines will have a -.
```
diff --git a/index.html b/index.html
index e69de29..f211c87 100644
--- a/index.html
+++ b/index.html
@@ -0,0 +1,9 @@
+<!DOCTYPE html>
+<html>
+<head>
+ <title></title>
+</head>
+<body>
+
+</body>
+</html>
\ No newline at end of file
```
15. Let's stage these changes using `git add .`. Run `git status` to now see that the files are listed as changes to be committed.
```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

  modified:   index.html

```
16. Lets create another commit for this change. Run `git commit -m "adding html structure to our index file"`.
17. Run `git log` and see that we have another entry in the log for our new commit
```
commit 789b028b04bfe0efc080827855dd00f79d20433d
Author: Eric Schwartz <eric@techtalentsouth.com>
Date:   Wed Feb 15 16:47:32 2017 -0500

    adding html structure to our index file

commit bddf8d41d0f8f6d0c9ebdb0677e2913b55da9311
Author: Eric Schwartz <eric@techtalentsouth.com>
Date:   Wed Feb 15 16:36:20 2017 -0500

    Initial commit
```
18. Run `git diff INITIAL_COMMIT_HASH SECOND_COMMIT_HASH` to see what was different between commits.