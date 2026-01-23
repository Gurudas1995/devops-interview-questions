# Git & GitHub Interview Questions and Answers

---
### Table of Contents

<details open>
<summary>
Hide/Show table of contents
</summary>

| No. | Questions |
| --- | --------- |
| 1   | [List All git Commands.](#List-All-git-Commands) |
| 2   | [What are the benefits of DevOps?](#what-are-the-benefits-of-devops) |
| 3   | [What is Continuous Integration?](#what-is-continuous-integration) |



1. ### List All git Commands.
Here are the list of common git commands which cover most important and frequently used operations.
- #### Setup and Configuration
  - `git config`: Configure user information like username and email.
  - `git config --global user.name "name"` : Set global username.
  - `git config --global user.email "email"` : Set global email.
- #### Initialization and Cloning
  - `git init`: Initalize a new Git Repository.
  - `git clone [url]`: Clone an existing repository from url.
- #### Snapshoting
  - `git status`: Show the working tree status.
  - `git add [file or .]`: Add specified or all changes to staging area.
  - `git commit -m "message"`: Commit staged changes with a message.
  - `git rm [file]`: Remove a file from the working directory and staging.
- #### Branching and merging
  - `git branch`:- List all local branches.
  - `git branch -a`: List all local and remote branches.
  - `git branch [branch-name]`: create a new branch.
  - `git branch -d [branch-name]`: Delete a local branch.
  - `git branch -D [branch-name]`: Force delete local branch.
  - `git checkout [branch-name]`: switch to a branch.
  - `git checkout -b [branch-name]`: Create and switch to a new branch.
  - `git merge [branch-name]`: Merge a branch into current branch.
  - `git checkout -- [file]`: Discard changes in a file.
- #### Remote Repositories
  - `git remote add origin [url]`: Add a remote repository.
  - `git remote set-url origin [url]`: Change the URL of a remote.
  - `git push origin [branch-name]`: Push a branch to a remote repository.
  - `git push -u origin [branch-name]`: Push and set upstream branch.
  - `git push origin --delete [branch-name]`: Delete a remote branch.
  - `git pull`: Fetch and merge changes from the remote repository.
  - `git pull origin [branch-name]`: Pull changes from a specific branch.
- #### Inspecting and comparing
  - `git log`: Show commit logs.
  - `git log --online`: Show summarized commit logs.
  - `git diff`: Show changes not staged.
  - `git diff --staged`: Show changes staged for commit.
  - `git show [commit]`: Show a commit details and changes.
- #### Undoing changes
  - `git reset [file]`: Unstage a file.
  - `git reset --hard [commit]`: Reset to a specific commit and discard changes.
  - `git revert [commit]`: Revert changes introduced by a commit.
- #### stashing
  - `git stash`: Stash changes in working direcotry.
  - `git stash pop`: Apply the latest stash.
  - `git stash clear`: Remove all stash entries.
    

**[⬆ Back to Top](#table-of-contents)**
