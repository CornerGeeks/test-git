## Introduction ##
- A distributed revision control system
- Doesn't require a central server to work
- Download at: http://git-scm.com/
- Command line tutorial below, execute code blocks in sequence
- Lines starting with # are command line outputs or notes 


### Configuration ###
    git config --global user.name "John Doe"
    git config --global user.email johndoe@example.com
    git config --list

      # user.name=John Doe
      # user.email=johndoe@example.com
      # -- other config

    git config user.name

      # John Doe

- user.name and user.email is what is recorded in commit logs


### Initialise ### 
Go to a folder in command line and initialise it as a git repository

    git init
    
      # # Initialized empty Git repository in /Users/user/test_git/.git/

### See current status and branch ### 

    git status
    
      # # On branch master
      # #
      # # Initial commit
      # #
      # nothing to commit (create/copy files and use "git add" to track)    

Will show current branch and status of files in the folder

### Save (Commit) files into the Repo ###
- Create a file & add it to the staging area

        touch README
        git add README

- See current status

        git status

          # # On branch master
          # #
          # # Initial commit
          # #
          # # Changes to be committed:
          # #   (use "git rm --cached <file>..." to unstage)
          # #
          # #    new file:   a
          # #

- Commit the file 
        git commit -m "Added README (C1)"

          # [master (root-commit) 93ecaa8] Added README (C1)
          #  1 file changed, 0 insertions(+), 0 deletions(-)
          #  create mode 100644 README

- Can also commit with "git commit" then type in the editor and save

### Status of Files ###
1. **Untracked**: Will not be saved / commited into the repo ("git add filename" to stage")
2. **Staged**: To be saved into the repo ("git commit" to save into repo and make it unmodified)
3. **Unmodified**: Previously committed. Currently has no changes
4. **Modified**: Previously committed. _Content has changed_. Stage file ("git add filename") in order to commit new version to repo

- Cannot commit empty folders into git


### Staging Files ###
- A file needs to be staged before it can be committed
- A staged file will stage the file at that point in time

        git status
      
          # # On branch master
          # nothing to commit, working directory clean 
        
        echo "line 1" >> README
        git status      
      
          # # On branch master
          # # Changes not staged for commit:
          # #   (use "git add <file>..." to update what will be committed)
          # #   (use "git checkout -- <file>..." to discard changes in working
          # directory)
          # #
          # #    modified:   README
          # #
          # no changes added to commit (use "git add" and/or "git commit -a")
      
        git add README
        echo "line 2" >> README
        git commit -m "commits only line 1 (C2)"
      
          # [master 58001d6] commits only line 1 (C2)
          #  1 file changed, 1 insertion(+)      
         
        git status
      
          # # On branch master
          # # Changes not staged for commit:
          # #   (use "git add <file>..." to update what will be committed)
          # #   (use "git checkout -- <file>..." to discard changes in working
          # directory)
          # #
          # #    modified:   README
          # #
          # no changes added to commit (use "git add" and/or "git commit -a")      

        git commit -am "auto stage tracked files and commit (C3)"
      
          # [master cbf5a24] auto stage tracked files and commit (C3)
          #  1 file changed, 1 insertion(+)


### Viewing previous commit logs ###

      git log
      git log -2
      git log --pretty=oneline
      git log --pretty="format:%h: %s (%an)"

### See previous commit states ###
-Checkout the state of the repo at any commit

      # git checkout previousCommitHashShownInGitLog
    
      git log --pretty=oneline
    
        # cbf5a24f41bb8a2f70527f5a96cea4fca411910b auto stage tracked files and
        # commit (C3)
        # 58001d6d9c474a6e5b102cf53a1948b0f5fd213d commits only line 1 (C2)
        # 93ecaa8c1238b872827781f6136401e4b9e3849b Added README (C1)
       
      cat README
      
        # line 1
        # line 2
          
      git checkout 25555caa7589363b20b3e1b34bd7ba979c13ff57
      cat README
      
        # line 1
          
      git checkout master
      cat README
      
        # line 1
        # line 2

- Think of commits as chain of events linked to the previous commit

        C1 - C2 - C3 <-- master <-- head

- master is the default branch and points to the last commit in the branch
- HEAD points to you current location in the commit chain (changes when you
  checkout)
- Checking out will move the head pointer and shows the state of the files at
  the point in time
- "git checkout master" will point the HEAD back to master which is the latest
  commit
- Note: All version of files are stored locally so you can git checkout without
  any network

### Ignoring Files ###
- Specify in .gitignore folder

      touch ignore_this_file
      git status

        # # On branch master
        # # Untracked files:
        # #   (use "git add <file>..." to include in what will be committed)
        # #
        # #    ignore_this_file
        # nothing added to commit but untracked files present (use "git add" to
        # track)

      echo "ignore_this_file" >> .gitignore
      git status

        # # On branch master
        # # Untracked files:
        # #   (use "git add <file>..." to include in what will be committed)
        # #
        # #    .gitignore


## Branches ## 
- When working on a new feature, create a feature branch, work on it and then
  merge it into the master branch

### View Branches ###
    git branch
       # * master

       # C1 - C2 - C3 <-- master <-- head

### Create New Branch ###

    git branch new_feature

      # C1 - C2 - C3 <-- master <-- head
      #           ^
      #           '---- new_feature
          
    git branch
      # * master
      #   new_feature

### Switch to new branch ###

    git checkout new_feature
      # Switched to branch 'new_feature'
        
      # C1 - C2 - C3 <-- master
      #           ^
      #           '---- new_feature <-- head

    git branch
      #   master
      # * new_feature
        
    echo "new feature" >> README
    
    git commit -am "new feature added (C4)"          
    
       # C1 - C2 - C3 <-- master
       #           |
       #           '---- C4 <-- new_feature <-- head


### Merging Branches ###
- Merge changes from new_feature into master

    git checkout master
    
      # C1 - C2 - C3 <-- master <-- head
      #           |
      #           '- C4 <-- new_feature

    git merge new_feature master
      # Updating e7578b4..13107de
      # Fast-forward
      #  README | 1 +
      #  1 file changed, 1 insertion(+)
    
      # C1 - C2 - C3 
      #           |
      #           '- C4 <-- new_feature 
      #              ^
      #              '---- master <-- head
- As master has not been changed, the master pointer just "fast forwards"

### Delete Branch after use ###

    git branch -d new_feature
    
      # C1 - C2 - C3
      #           |
      #           '- C4
      #              ^
      #              '---- master <-- head


### Merge Conflicts ### 
- Happens when conflicting changes happen between merges

    C1 - C2 - C3 - C4 <-- master

- Create new branch and switch to it (shortcut)
    git checkout -b merge_conflict

      echo "merge_conflict_branch" > README
    git commit -am "one line README in merge_conflict (C5)"
      
    # C1 - C2 - C3 - C4 <-- master
    #                |
    #                C5 <-- merge_conflict  <-- head

    git checkout master
    echo "master add new line" >> README
    git commit -am "yet another line (C6)"

    # C1 - C2 - C3 - C4 - C6 <-- master  <-- head
    #                |
    #                C5 <-- merge_conflict

    git merge merge_conflict master
      
    # Auto-merging README
    # CONFLICT (content): Merge conflict in README
    # Automatic merge failed; fix conflicts and then commit the result.

- Will now need to edit the README file manually to resolve conflicts
 
      cat README

        # <<<<<<< HEAD
        #  line 1
        #  line 2
        #  new feature
        #  master add new line
        #  =======
        #  merge_conflict_branch
        #  >>>>>>> merge_conflict

- New file shows the changes between the branches.
- Delete and clean the <<<<<<, =======, >>>>>> and text in between.
   - "<<<<<<<" : start of your current branch's file
   - "=======" : end of your current branch's file & start of the difference
     from other (merged) branch changes
   - ">>>>>>>" : end of other branch changes


      git commit -am "merged merge_conflict with master (C7)"

        # C1 - C2 - C3 - C4 - C6 -C7 <-- master  <-- head
        #                |
        #                C5 <-- merge_conflict

## Show changes ##
- Changes that haven't been staged

    echo "diff me" >> README
    git diff
    
      # diff --git a/README b/README
      # index 4b7c1e3..f6a1b72 100644
      # --- a/README
      # +++ b/README
      # @@ -4,3 +4,4 @@ new feature
      #  master add new line
      #  merge_conflict_branch
      # +diff me
        
- Changes that have been staged 

    echo "diff me again" >> README    
    git diff --cached    
        #

    git add README
    git diff --cached
        # diff --git a/README b/README
        # index 4b7c1e3..fbe856c 100644
        # --- a/README
        # +++ b/README
        # @@ -4,3 +4,5 @@ new feature
        #  master add new line
        #  merge_conflict_branch
        # +diff me
        # +diff me again

## Unstage a file ##

    echo "to be unstaged" >> README
    git add README
    git status
    
      # On branch master
      # Changes to be committed:
      #   (use "git reset HEAD <file>..." to unstage)
      #
      #    modified:   README
      #    
      
    git reset HEAD README
    
      # Unstaged changes after reset:
      # M    README    
      
    git status
    
      # # On branch master
      # # Changes not staged for commit:
      # #   (use "git add <file>..." to update what will be committed)
      # #   (use "git checkout -- <file>..." to discard changes in working
      # directory)
      # #
      # #    modified:   README
      # #
      # no changes added to commit (use "git add" and/or "git commit -a")

## Restore file to previous version ##
    echo "these changes will be reset" >> README
    git status
    
      # # On branch master
      # # Changes not staged for commit:
      # #   (use "git add <file>..." to update what will be committed)
      # #   (use "git checkout -- <file>..." to discard changes in working
      # directory)
      # #
      # #    modified:   README
      # #
      # no changes added to commit (use "git add" and/or "git commit -a")
      
    git checkout -- README
    git status
    
      # # On branch master
      # nothing to commit, working directory clean    

## Reset all tracked files to previous commit ##

    git reset --hard

- Deletes all changes since last commit
- Only affects tracked files
- Untracked files do not get deleted


## Working with remotes ## 
- (e.g. GitHub, your own server, etc) 

### Push Changes to Remote ###
- Add a remote to you bare repo
- "git push repo_name branch_to_push"

        git remote -v
            # 
        git init ../git.git --bare
        git remote add my_repo ../git.git 
        git remote -v
    
            # my_repo  ../git.git (fetch)
            # my_repo  ../git.git (push)
      
        git push my_repo master
    
            # Counting objects: 24, done.
            # Delta compression using up to 2 threads.
            # Compressing objects: 100% (14/14), done.
            # Writing objects: 100% (24/24), 2.07 KiB | 0 bytes/s, done.
            # Total 24 (delta 1), reused 0 (delta 0)
            # To ../git.git
            #  * [new branch]      master -> master

### Clone from a remote repo ###

        git clone ../git.git ../git_clone
        cd ../git_clone
        git remote -v
            # origin    /Users/user/test_git/../git.git (fetch)
            # origin    /Users/user/test_git/../git.git (push)      

- "origin" is the default name of your git repo that you have access to

### Download changes from a Remote URL ###

        git fetch origin
        # git fetch remote_name

- Does not merge into your existing code, just downloads all changes locally so
  can merge / checkout

### Merge Changes from Remote Branch ###
        git fetch origin
        git merge origin/master master
            #  **fix any conficts if needed and commit**
            # can also do "git pull origin"

## Workflow for pushing to a Repo ##
- Clone from Repo
      git clone url_to_git_repo
- Go into directory and make changes
      cd url_to_git_repo
      echo "cloned" >> README
- Commit your changes
      git commit -am "my changes"
- Fetch and merge from origin
      git pull origin
- Fix conflicts (if any)
      git commit -am "merged origin to masteR"
- Push to origin
      git push origin master

### GitHub Pull Requests ###
- Fork a project
- Clone locally
- Add remote to project you forked from (typically called "upstream")
      git remote add upstream git.repo
- Make your changes
- Make your commit
    git pull upstream/master master
    *resolve conflicts*
    git commit
- Use web interface to create pull request
- Go to your fork
- Sidebar > Pull Request
