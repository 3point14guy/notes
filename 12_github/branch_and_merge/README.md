# Exercise - Create and merge a branch
In this exercise we will create a new branch from master. We will make a small change to a file in our new branch and then merge it back into the master branch. When we have finished the merge, we will delete our feature branch.

***Branching***
Lets go ahead and practice branching by making a simple branch and modifying our index.html file.

* Run `git checkout -b AddHeader` to create a branch called AddHeader and checkout that branch
* Run `git status` and notice that you are now on the AddHeader branch
```
On branch AddHeader
nothing to commit, working directory clean
```
* Open up index.html and add a header using the `<h1>` 
* Run `git status` to show that we are on our branch and that the index.html file has been modified.
```
On branch AddHeader
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

  modified:   index.html

no changes added to commit (use "git add" and/or "git commit -a")
```
* Run `git add index.html` to stage this file for commit
* Run `git commit -m "adding a heading to the website"` 
* Run `git log` and now you can see that new commit has been added
* Checkout the master branch with `git checkout master`
* Now look at the contents of ***index.html***. You should notice your header you added is gone. This is because that change only lives on your branch.
* Checkout the AddHeader Branch, `git checkout AddHeader`, and you should see your header again.

***Merging***
Now that we have made our simple change and can see that our code can vary from branch to branch, lets get our changes from our feature branch merged into our master branch. Once merged we can delete our branch.

**NOTE** ***You always want to merge master (or whatever branch you want to put your work into) into your branch first. This will help avoid merge conflicts in the master branch. This way we can clean up issues in our feature branch and then merge the clean code into master.***

* Make sure you are on the AddHeader branch, `git checkout AddHeader`
* Run `git merge master`. This will take any changes that have been made to master since you created the branch and add them to your branch.
* Now checkout master, `git checkout master`.
* Merge the AddHeader branch into the master branch, `git merge AddHeader`.
* This may popup a text editor for you to provide a message for the merge commit. Go ahead and save that file. 
* Now open the index.html file while on the master branch and notice the header has been added.
* Now you can delete your feature branch with `git branch -d AddHeader`


