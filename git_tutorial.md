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
* 