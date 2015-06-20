# Git Tutorial


Git can be used on MacOS, Linux and Windows. This Tutorial focuses solely on MacOS.


## Git Installation




## Initialising directory

* At the root of the project, run  

	```git init```


## Commit
* Add all files in the project that are to be committe	

	```git add .```

* Add only one file **`hadoop_installation_guide.md`**
	
	```git add hadoop_installation_guide.md```

* Commit the files
	
	```git commit```
	

## Logging

* You can check who has committed and their commit messages and what has changed.

	```git log```	

* Get the help file

	```git help log```	

* The last commit and last 5 commits only	

	```git log -n 1```

	```git log -n 5```

### Log and search by date
* Everything since that day and everything after

	```git log --since=2015-06-18```
	
	
* Everything from the beginning up until 
	
	```git log --until=2015-06-15```	
	

* Both since and until, in between a certain range
	
	```git log --since=2015-06-15 --until=2015-06-18```


### Log and search by author	

`git log --author="Matthew Lai"`


### Log and search by Grep


```git log --grep="create"```

* You can do this to look for only bug fixes commits.


## Architecture (Optional)

### Two-tree architecture	

* The concept of **Repository** and **Working** directory using **'commit'** and **'checkout'**


### Three-tree architecture

* From **Working** to **Staging index**, use

	`git add file.txt`

* From **Staging index** to **repository**, use

	`git commit file.txt`


* I can save all my files in theh working directory, and only commit one file to respostiory


* We will pull everything from respository to working drectory.


## Git workflow

## Git HEAD pointer (Optional)
* You can switch from **Master** brunch to a different brunch
* To keep track of the **HEAD**
* To see which brunch the **HEAD** is poinint, run this

	```
	$ cat ~/.git/HEAD
	ref: refs/heads/master
	```
	This indicates the HEAD is looking at the **Master** brunch.

* To see which commit the **HEAD** is on, run this

	```
	$ cat ~/.git/refs/heads/master
	43c01ad50f3d2269a5306b3d5585d066e2c3ee58
	```

	This shows the commit ID where the **HEAD** is on. 
		
* Alternatively, you can see the tip of the current brunch by 

	```$ git log HEAD```
	
	
## Make changes to Files
This section is a workflow, we must follow each steps

1. The status of 3 different brunches

	`$ git status` and we get
	
	
	```
	On branch master
	nothing to commit, working directory clean
	```
	This means we are working on the Master brunch and we have the latest, there is nothing to commit
	
2. If we create a new file in the directory, and do `$ git status`, we get 

	```
	On branch master
	Untracked files:
	  (use "git add <file>..." to include in what will be committed)

		git_tutorial.md

	nothing added to commit but untracked files present (use "git add" to track)
	```
3. This means, gif is not tracking `git_tutorial.md`, so to make sure we can track it, we will add

	`$ git add git_tutorial.md`

4. To add multiple files, one follows another

	`$ git add git_tutorial.md second_file.txt third_file.md`

5. or we can 

	`$ git add .`

6. To confirm, run `$ git status` again, and get the following

	```
	On branch master
	Changes to be committed:
	  (use "git reset HEAD <file>..." to unstage)

		new file:   git_tutorial.md
	```	
7. We should get this, since **`hive_installation_guide.md`** was not added and it is not tracked.

	```
	On branch master
	Changes to be committed:
	  (use "git reset HEAD <file>..." to unstage)

		new file:   git_tutorial.md

	Untracked files:
	  (use "git add <file>..." to include in what will be committed)

		hive_installation_guide.md
	```

8. We will have to also add **`hive_installation_guide.md`** 

	`$ git add hive_installation_guide.md`

9. But we can still commit the one file that was added. 

	`$ git commit -m "create git_tutorial.md"`	

10. Always remember to run `$ git status`

## Meaning of **`$ git status`**

	```
	On branch master
	Changes not staged for commit:
	  (use "git add <file>..." to update what will be committed)
	  (use "git checkout -- <file>..." to discard changes in working directory)

		modified:   git_tutorial.md

	Untracked files:
	  (use "git add <file>..." to include in what will be committed)

		hive_installation_guide.md

	no changes added to commit (use "git add" and/or "git commit -a")
	```

* This means **`hive_installation_guide.md`** has not been added at all to staging
* **git_tutorial.md** was added and committed, but now, it has been edited in the working directory, so it says it is not staged.
	* **modified**: means it is in staging, we will need to commit it.
	* **untracked**: means it is in working, not even added to staging yet.

## Meaning of modification **`$ git diff`**

* **`$ git status`** tells us which file has been modified, but we do not know what has actually been modifeid.
* we use diff in unix, we do something similar by comparing stating and working directory `$git diff`
* It tells us what has been added and what has been changed.
* **_Red_** = -removed, **_Green_** = +added
* It only shows a bit of the chnage, not the whole line that is changed..
* ```@@ -1,4 +1,3 @@``` tells us line one and the next 4 lines are removed and replaced by line 1 and next 3 lines from the new file.
* You can diff one file `$ git diff git_tutorial.md`
* `$ git diff` comparing working and staging, as soon as it is added, there won't be a difference
* `$ git diff --staged`, comparing staging and respository, what has been staged and what has been committed.


## Deleting files
* Supposed to you added your unwanted files to staged by `$git add...`
* and then have them committed to respository `$ git commit -m ""`

###Method 1
* physically remove the file `file_to_delete_1.txt` from your working directory
* then do `$ git status`, tells us something has been deleted
	
	```
	On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:   file_to_delete_1.txt
	```

* then do `$ git add file_to_delete_1.txt`, 
* Then do `$ git status` and should say 

	```
	On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	deleted:    file_to_delete_1.txt
	```

* the deleted file has not been commited.
* Then do `$ git commit -m "removed file_to_delete_1.txt"`, it should say

	```
	[master ba8bc3b] remove file_to_delete_1.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 file_to_delete_1.txt
 ```
* we see the fiile is now commited and it actually deletes what is in the repository.
	
* To confirm,  do `$git log`, will see what has been commmited.


### Method 2 

* Do not physically remove the file, and follow these steps
* run `$ git rm file_to_delete_2.txt` 
* Then `$ git status`
* In one step, it is already in staging, we don't have to do add, method 1 is a 2 step processes.

	```
	On branch master
	Changes to be committed:
	  (use "git reset HEAD <file>..." to unstage)

		deleted:    file_to_delete_2.txt
	```
* Now, we can do commit `$ git commit -m "remove file_to_delete_2.txt"`
* It should say

	```
	[master de60864] remove file_to_delete_2.txt
	 1 file changed, 1 deletion(-)
	 delete mode 100644 file_to_delete_2.txt
 
 
* Now, try `$ git log`, will say what has been commited.



## Renaming

### Mehod 1
* Physically rename an existing file that was already commited.
* `$git status`

	```
	On branch master
	Changes not staged for commit:
	  (use "git add/rm <file>..." to update what will be committed)
	  (use "git checkout -- <file>..." to discard changes in working directory)

		deleted:    git_tutorial.md

	Untracked files:
	  (use "git add <file>..." to include in what will be committed)

		git_tutorial_1.md
	```

* It should actually tell us a file is removed and a new file is created an untracked

* `$ git add git_tutorial_1.md` adding the NEW file
* `$ git rm git_tutorial.md` remove the OLD file
* `$ git status`

	```
	On branch master
	Changes to be committed:
	  (use "git reset HEAD <file>..." to unstage)

		renamed:    git_tutorial.md -> git_tutorial_1.md
	```		
	
* now you know it has been removed.
* As long as the files are 50% similar, then it is considered a rename

### Method 2
* `$ git mv git_tutorial_1.md git_tutorial.md`, no need to physically to rename files, just use mv command
* `$ git status`

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	renamed:    git_tutorial.md -> git_tutorial_1.md
```
* the same thing and ready to be committed.
* This is one step process.
* Let's say you remove the existing file to a new folder, we still use
* physicall create a directory called git
* `$ git mv git_tutorial.md git/git_tutorial.md`
* `$ git status`

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	renamed:    git_tutorial.md -> git/git_tutorial.md
```	
* `$ git commit -m "Reorganisesd file structure"`

## Undo changes

### Working directory
* If we didn't mean to delete teh files
* `git status` it tells us what has been modeifed
* `git diff` tells you hte changes
* you will see Red and -
* So, if we want the respository version back. and replace what I have 
* `git checkout index.html`
* but checkout can do the brunch as well... dangerours
* `git checkout -- index.html`, we are just talking about a file at the current brunch
* `git status`

### Stating directory
* undo in the staging index
* Let's mess up the staging
* `$ git add git_tutorial.md` where it is a corrupted file

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   git_tutorial.md
```
* You siill want all your changes in your working directory
* you add in something you didn't want that is to be committed to respository, but it is still someething you want to have in your working directory. maybe something you don't want yet people to see it.
* `$git reset HEAD git_tutorial.md` go to the last commit, where the HEAD is pointing at and bring it in.

```
Unstaged changes after reset:
M	git_tutorial.md
```

* `$ git status`

```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   git_tutorial.md
```	

### Undo commit - replace the last commit
* This is tricky and sometimes what I wanted to do.
* We can only chnage thet last commit, because nothing else depends on it. that is.
* We continue to work on it
* `git commit --amend -m` "We want to undo the last commit"
* All the commit amend do is to replace the last commit with this new one, that is all. You are not removing the last commit, just replace it
* So, we will still need to add and commit first.


### Undo commit - Method 2
* Another way is to really checkout the version that we want and then commit.
* Checkout the version you like `$ git checkout fb485686247c5483088 -- hive_installation_guide.md`
* you will now have this version of hive_isntallation_guide.md in your working directory.
* `$git commit -m "use this hive_installation.md from this version instead"`
* You can't do --amend here because it will look exactly the same as in the previous version.
* If you do ammend, then you must not used the last previous commit, otherwise it is empty.

### Undo commit - Method 3
* this may be best - revert, that is to revert back before we commit
* Find the veersion you need, `git revert def4b34e354c6991c6942a4d712d1b09638e717a`
* this is to bring to your working directory as well as to commit the changes all into resposiotry in one step
* you are replacing everything in that version to its previous state.
* `git status` 

	```
    Revert "trying"
    
    This reverts commit cd9329e88c4be3f5c600d65f274547f1df1fe6de.
	```    

 
* you have gone back to its previous committed state, which is still there.


### Undo multiple commits

* `git reset`
* we usually let git to move the HEAD pointer
* now, we are in control
* we can go to a commit and take it from there, which will overwrites all the rest of the commit, actually I might like this.
	* soft - doesn't change staging and working at the same time, safest.
	* mixed - change staging as well, but not with working directory
	* hard -- change everything, whatever it is.

### Some reset

* take a look at the HEAD poniter
* `cat .git/HEAD` - showing master brunch
* `cat .git/refs/heads`
* 'git reset --soft e0c2fd6b4c3558e4158310a8638f3380529dea53` will take you back to the last stable version, but nothing else in your working or staging is changed.
* You can change the HEAD pointer
* git rest --soft 065e974b2ba217f4c82af386e68cc54a5b7be1ac, the last currupted one
* then you can do another commit
* 

* you can do mixed reset and hard reset.. but I am skipping them for now.

### Remove untracked files from working directory
* usually the log files compiled directory whatever, you want them too
* `git clean`
* It is like this
* 'git status'

	```
	Untracked files:
	  (use "git add <file>..." to include in what will be committed)

		junk.md
	```	 
* You can see junk.md is the untracked file
* `git clean -n`	, and we get this
``` Would remove junk.md```
* `git clean -f` forces it to run
```Removing junk.md```
* but it also removes it physiclaly, so same as rm
* but if it is already in the staging directory, it won't be removed.
* to unstage, `git reset HEAD junk.md`, this is to undo all the add junk.md
* Everything is permantly deleted


## Ignore files


## Branching

* Instead of making classes and your new ideas and remove it from master, you can just work on a burnch until you are happy with it.
* Eventually, merge it to Master
* Just one working directory
* You can switch HEAD to Master brunch or your brunch
* The HEAD is always at the last commit wherever you switch brunch

### To see brunches
* to see brunches `git branch`
* `cat .git/HEAD`

	```ref: refs/heads/master```

* That is just what brunch you are on, which is master
* To see if this is master, you can
* `ls -la .git/refs/heads`, you should see this

	```
	drwxr-xr-x  3 emelle  staff  102 19 Jun 18:44 .
	drwxr-xr-x  4 emelle  staff  136 18 Jun 16:37 ..
	-rw-r--r--  1 emelle  staff   41 19 Jun 18:44 master
	192-168-1-109:mink-0.0.1 emelle$ 
	```

* when we do brunch, it will ad a new brunch there
* The sha is in
* `cat .git/refs/heads/master`, it may say `998d1bf0b5c49e218c3e493ee3f6786a9ced034e`
* and to confirm do `git log --online`

	```
	998d1bf Another stable version
	065e974 Completely wrong stuffs commits
	.
	.
	.
	so on
	```
* They both share the master sha
* To create a new brunch
* `git brunch new_feature`
* `git brunch`, you will get

	```
	* master
  new_feature
192-168-1-109:m
```

* but we are still on the Master branch
* `ls -la .git/refs/heads`

```
drwxr-xr-x  4 emelle  staff  136 20 Jun 12:32 .
drwxr-xr-x  4 emelle  staff  136 18 Jun 16:37 ..
-rw-r--r--  1 emelle  staff   41 19 Jun 18:44 master
-rw-r--r--  1 emelle  staff   41 20 Jun 12:32 new_feature
```

* but the two bracnhes are still pointing at the same commit

* `cat .git/refs/heads/new_feature`, it is the same as `998d1bf0b5c49e218c3e493ee3f6786a9ced034e`

* Is it still pointing to Master?
* `cat .git/HEADS` still points to master `ref: refs/heads/master`
*

### To switch branch

* `git checkout new_feature`, you will get

```
M	git_tutorial.md
Switched to branch 'new_feature'
```
* `cat .git/HEAD` points at `ref: refs/heads/new_feature`
* but they are still pointing at the same commit with same SHA
*  `git commit -am "latest branching"`, you can do both at once
*  
