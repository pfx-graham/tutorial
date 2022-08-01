Git Tutorial: Notes and plan on a general walkthrough.
======================================================
`export PS1='[\u@\h]\$ '`
`exec sh`

# Reference
- [Github Doucmentation](https://docs.github.com/en)
- [Github Quickstart](https://docs.github.com/en/get-started/quickstart/hello-world)
- [Connecting to Github with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [Github Cheat Sheet PDF](https://training.github.com/downloads/github-git-cheat-sheet.pdf)
- [Some good general usage tips](https://public-health-scotland.github.io/git-guide/reference.html)
- [Pro Git Book (Freely Available Digital Copy))](https://git-scm.com/book/en/v2)

# Git vs Github: what's the difference
[Git vs. GitHub: What is the difference between them?](https://www.theserverside.com/video/Git-vs-GitHub-What-is-the-difference-between-them)
>Git is a distributed version control tool that can manage a development project's source code history, while GitHub is a cloud based platform built around the Git tool. Git is a tool a developer installs locally on their computer, while GitHub is an online service that stores code pushed to it from computers running the Git tool.

# Git Client Setup
## Generating a Public SSH Keypair, then adding it your Github account
1. `ssh-keygen -o`
2. `cat ~/.ssh/id_rsa.pub`
3. [Generating a new SSH key and adding it to the SSH agent](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

## First Time Configuration
[Local Configuration](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

View all of your settings and where they are coming from using:
1. `git config --list --show-origin`

Given name assocaited with your commits
2. `git config --global user.name "John Doe"`

Email address associated with your commits
3. `git config --global user.email johndoe@example.com`

Specify text editor used for commit messages
4. `git config --global core.editor nano`

# Essential Concepts

## Creating a repository, adding and removing files to the staging area
`mkdir project`

`cd project`

`git init`

`git add <example_file1.txt>`

`git commit`

`git log`

`git log --oneline`

`git add <example_file2.txt>`

`git commit -m "Summary of changes made, why changes were introduced."`

`git rm <example_file2.txt>`

`git commit -m "Removed example_file2.txt to serve as an example of removing a file."`

`git log`

## Pushing one or more commits to a remote system, decentralized version control
`git remote add origin https://github.com/repo_name/sample-repo.git`

`git branch`

`git push origin master`

...in a seperate directory
`git clone https://github.com/repo_name/sample-repo.git`

`cd sample-repo`

`git log`

![DVCS](https://latesthackingnews.com/wp-content/uploads/2018/06/Distributed-Version-Control-System-Workflow-What-Is-Git-Edureka.png "Git is a decentralized version control system")

## Reverting/rolling-back a previous commit, reverting group of commits
## git revert
The revert command will create a commit that reverts the changes of the commit being targeted.
`git revert <hash of commit to revert>`

## git reset
You can also use the reset command to undo your last commit. But be careful – it will change the commit history, so you should use it rarely. It will move the HEAD, the working branch, to the indicated commit, and discard anything after
`git reset --soft HEAD~1`

>The --soft option means that you will not loose the uncommitted changes you may have.

>If you want to reset to the last commit and also remove all unstaged changes, you can use the --hard option.

## revert vs reset: what's the difference, when to use each
You should really only use reset if the commit being reset only exists locally. This command changes the commit history and it might overwrite history that remote team members depend on.

revert instead creates a new commit that undoes the changes, so if the commit to revert has already been pushed to a shared repository, it is best to use revert as it doesn't overwrite commit history.

## Handling sensitive information, excluding files and paths
[Git ignore](https://www.atlassian.com/git/tutorials/saving-changes/gitignore)
>If you want to ignore a file that you've committed in the past, you'll need to delete the file from your repository and then add a .gitignore rule for it. Using the --cached option with git rm means that the file will be deleted from your repository, but will remain in your working directory as an ignored file.

1. `echo debug.log >> .gitignore`
2. `git rm --cached debug.log`
3. `git commit -m "Start ignoring debug.log"`

## Editing and re-ordering commits, organizing previous commits
[Editing and reordering commits via interactive rebase](https://www.sitepoint.com/git-interactive-rebase-guide/)

`git rebase -i <quantity of commits to edit>`

`git rebase -i HEAD~3`
Is an example use case of rebasing the three most recent commits on the current branch.

>Interactive Rebase helps you optimize and clean up your commit history. It covers many different use cases, some of which allow you to to the following.
- edit an old commit message
- delete a commit
- squash/combine multiple commits
- reorder commits
- fix old commits
- split/reopen old commits for editing

>Like a couple of other Git tools, interactive rebase “rewrites history”. This means that, when you manipulate a series of commits using interactive rebase, this part of your commit history will be rewritten: the commit’s SHA-1 hashes will have changed. They’re completely new commit objects, so to speak.

## Handling merge conflicts, techniques to avoid merge conflicts
### Handling conflicts
[Handling merge conflicts example](https://www.atlassian.com/git/tutorials/using-branches/merge-conflicts)
1. `mkdir git-merge-test`
2. `cd git-merge-test`
3. `echo "this is some content to mess with" > merge.txt`
4. `git add merge.txt`
5. `git commit -am"we are commiting the inital content"`
6. `git checkout -b new_branch_to_merge_later`
7. `echo "totally different content to merge later" > merge.txt`
8. `git commit -am"edited the content of merge.txt to cause a conflict"`
9. `git checkout master`
10. `echo "content to append" >> merge.txt`
11. `git commit -am"appended content to merge.txt"`
12. `git merge new_branch_to_merge_later`

Auto-merging merge.txt
CONFLICT (content): Merge conflict in merge.txt
Automatic merge failed; fix conflicts and then commit the result.

$ cat merge.txt
<<<<<<< HEAD
this is some content to mess with
content to append
=======
totally different content to merge later
>>>>>>> new_branch_to_merge_later

### Preventing merge conflicts
In order to avoid a merge conflict, all changes must be on different lines, or in different files, which makes the merge simple for computers to resolve. In other words, if a change introduces any ambiguity even at a single line of code an automatic merging is canceled and the whole process must be finished manually.

1. Whenever it is possible, use a new file in preference to an existing one. The only ambiguity could happen is the same name and path of the file

2. Do not always put your changes at the end of a file. Decreases the probability of editing the same line of code

3. Do not organise imports. Decreases the probability of editing the same line of code

4. Do not beautify a code outside of your changes. Decreases the probability of editing the same line of code

5. Push and pull changes as often as you can. Mitigates distributed nature of Git

# Linear Workflow
The most popular and the entry stage of every project (at least initially).

There is one central repository. Each contributor clones the repository, works locally on the code, creates a commit with changes, and pushes it to the central repository for other contributors to pull and use in their work.

# Feature Branch Workflow
Once two or more contributors start working on separate functionalities within one repository, problems begin to appear.

For example, one of the contributors finished their functionality and wants to release it. However, the second feature isn’t ready. Making a release at this moment presents a challenge due to the second feature blocking the first.

Feature branches are independent “tracks” of project development. For each new functionality, a new branch is created, where the new feature is developed and tested. Once the feature is ready, the branch can be merged to the master branch so it can be released to the production server.

# Forking Workflow
Any time a contributor wants to change something, they don’t work directly on the project's repository. Instead, they fork it, creating a copy of the entire repository. Then, the contributor is free to work on the new feature in whatever fashion they please. An individual or company can fork a repository and take the project in an entirely new direction which wouldn't be allowed by the original team members.

In most of cases, however, when the work is done, a pull request is created, which compares the changes the contributor introduced to their fork to the state of the forked repository. From there, the community and the project's owners can review, discuss, and test the changes. The final call remains in the hands of the project's leader and their team members.

At its core, forking is similar to feature branching. Instead of creating branches, a fork of the repository is made, and instead of working with merge requests you create pull requests, which are a way of asking to include your changes in the forked project.
