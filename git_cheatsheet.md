# Git

A *distributed version control system* that focuses on changesets. 

## Topologies

**Centralized**
- Developers push to one main repository

**Hierarchical**
- Developers push changes to subsystem repositories, which are periodically merged into a main repository

**Distributed**
- Developers push changes to their own repositories, where changes will be pulled into the main repository
- Each repository acts as a backup to the main repository

## Git Commands

_Note:_ All commands will be prefaced with the `git` command (git <command>)

|Command|Description|Format|Example|
|:-:|:-|:-:|:-|
|add|Add/update files to the staging area to be tracked by git|`git add <files>`|git add -u|
|branch|Display, create, remove branches of a repository|`git branch`|git branch|
|cherry-pick|Select a single commit and apply to a branch|`git cherry-pick <commit-ID>`|git cherry-pick 223ea3f|
|clean|Will remove untracked files from the working copy unless specified otherwise|`git clean <options>`|git clean|
|clone|Download the history of a remote repository|`git clone <url>`|git clone https://github.com/Gily-H/references.git|
|commit|Save changes from the staging area into the local repository|`git commit`|git commit -m "message"|
|config|Access and modify the different levels of configurations for git|`git config <options>`|git config --global user.name "name"|
|diff|View changes made between commits|`git diff <older-commit-id>..<newer-commit-id>`|git diff HEAD~2..HEAD|
|fetch|Brings down the changes from the specified remote repository without merging|`git fetch <remote>`|git fetch upstream|
|init|Initialize a git repository|`git init ./`|git init ./references|
|log|View a history of the commits for the repository|`git log`|git log --oneline|
|merge|Merge changes into your local branch|`git merge <remote> <local>`|git merge origin/main|
|pull|Fetch and merge the changes from a remote branch into your local branch|`git pull <remote> <local>`|git pull upstream main|
|rebase|Replay a branch's commits and append them to another branch|`git rebase <base-branch>`|git rebase main|
|reflog|Display a history of commits that the **HEAD** had previously pointed to|`git reflog`|git reflog|
|remote|Used to manage a set of tracked repositories|`git remote <options>`|git remote -v|
|reset|Revert the working copy to to a previous commit or to latest HEAD commit by default|`git reset`|git reset --hard|
|restore|Discard changes in the working directory|`git restore <file>`|git restore mistake.txt|
|shortlog|Summary view of the commit history by author and commits|`git shortlog`|git shortlog|
|show|Provides a view of various objects like commits|`git show <object>`|git show HEAD|
|stash|Save the changes and reset the working copy to unmodified state|`git stash`|git stash|
|status|Check the status of tracked/untracked files|`git status`|git status
|tag|View the tags of the repository|`git tag`|git tag|

## Git Protocols
TODO

## Git Configuration

**System-Level**
System configuration applies to the machine that git is installed on. The configuration file can normally be found at `/etc/gitconfig`

```bash
# see the configuration values at the system level
git config --system --list
```

**User-Level**
Global configuration for a specific user. The configuration file can normally be found at `~/.gitconfig`

```bash
# see configuration values at the user level
git config --global --list

# add a configuration (key, value) for username at the user-level 
git config --global user.name "my name"

# remove a configuration at the user-level (using key) - # Will remove the most recent entry if there are duplicate configurations
git config --global --unset user.name
```

**Repository-Level**
Configurations specific to the repository where the config file is located in. The config file can be found in `.git/config` of the repository

```bash
# see the configuration values of the associated repository
git config --list
```

*Note:* Repository-level configurations will override User-level configurations, and User-level configurations will override System-level configurations

[Here](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration) is a list of some of the different git configurations that can be set

**Aliases**
We can create aliases (shorthand commands) in our configuration files for long git commands using the following format: `git config --<config-level> alias.<alias-name> "command-to-alias"`

```bash
# add the alias in the configuration file under the section for aliases
git config --global alias.lga "log --oneline --graph --all --decorate"

# run the alias
git lga
```

## Git Reset

**--soft**
Git will point the HEAD back to the designated commit. Changes that were made before the reset will be in the _staging area_ and can be recommitted using `git commit`

**--mixed**
Git will point the HEAD back to the designated commit. Changes that were made before the reset will be in the _working copy_ as untracked files. They can be readded with `git add` and recommitted using `git commit`

**--hard**
Git will point the HEAD back to the designated commit. Changes that were made before the reset will be gone.

```bash
# keep the changes in the staging area
git reset --soft <commit-id>

# keep the changes as untracked files
git reset --mixed <commit-id>

# completely remove the changes made after the reset commit
git reset --hard <commit-id>
```

## Git clean
Git will remove any untracked files from the working copy

```bash
# git clean requires an additional option before it can execute properly
git clean <option>

# -n, do a dry run to show what files would be removed before actually removing the files
git clean -n

# -f, force remove the untracked files
git clean -f
```

## Git log
Some useful flag options are outlined below

```bash
# display commits as one line - <abbr-hash><commit-message>
git log --oneline

# view commits as a graph outlining branches and merges
git log --graph

# view all branches
git log --all

# view all decorators (tags, branches)
git log --decorate

# display a summary view of the commit history - author and commits
git shortlog

# show diff of given commit
git show HEAD~1
``` 

## Branches
Branches are versatile in that they can be used to develop new features, fix bugs, save backups, etc.

```bash
# display all local branches
git branch

# display all remote branches of the repository
git branch -r

# create a branch from a commit
git branch <branch-name> <optional-commitID/tag>

# rename a branch
git branch -m <branch> <new-branch-name>

# force delete a local branch
git branch -D <branch-name>

# delete a remote branch
git push origin :<remote-branch-name>

# set the local branch to mirror an upstream remote branch
git branch --set-upstream <local-branch> <remote-branch>
```

## Tags
Points in the repository's history that have special designations (versions, stable releases, etc). Git does not push tags by default, requires the `--push` option

```bash
# view the tags of the repository
git tag

# create a tag for the repository
git tag <tag-name>

# annotate a tag for the repository - will be prompted to write a message (you can use the -m option)
git tag -a <tag-name>

# create a signed tag - requires a message - will be prompted to create a passphrase - provides signature verification of who created the tag
git tag -s <tag-name>

# verify a tag
git tag -v <tag-name>

# pushing tags to the remote repository
git push --tags
```

## Fetch & Merge
Fetching from a remote repository does not merge the changes into your local working copy. To update the local working copy, you would need to `git merge` the remote into your desired branch after fetching. Format for a git merge `git merge <remote-name> <local branch>`. Performing a `git pull` combines the actions of fetching and merging

```bash
# fetch the changes from the remote upstream (assuming a remote named 'upstream' was set)
git fetch upstream

# merge the changes brought down from the upstream remote repository
git merge upstream/master main

# pull - shortcut for fetching and merging
git pull upstream/master main
```

## Retrieving Deleted Commits
We can retrieve previous commits that were deleted. We would need to use the `git reflog` command (typically within a 30day period) to see where our **HEAD**had pointed to previously

```bash
# reflog will display a history of previous commits that the HEAD had pointed to (even deleted commits)
git reflog

# create a branch for the commit-ID
git branch <branch-name> <commit-ID>
```

## Stash
We can save unfinished changes to the stash and reset our working copy to a clean state (unmodified). We can then bring back the stashed changes at a later time. Changes will be applied in a last in first out (LIFO) policy

```bash
# save pending changes made in the working copy to the stash list
git stash

# View the stash list
git stash list

# Reapply the most recent stashed changes (the stash will still contain the changes)
git stash apply

# Remove the most recent change in the stash list
git stash drop

# Shorthand for git stash apply & git stash drop
git stash pop

# create a branch and apply the stash - this will pop the changes off the stash list
git stash branch <branch-name>
``` 

## Rebase
Replay the commits from one branch and append them to a base branch. This will make it appear as though the new branch's commits were added onto the base branch as regular commits

```bash
# rebase from feature branch to main branch
git rebase main

# if rebasing causes merge conflicts, resolve the conflicts and continue the rebase
git rebase --continue
```
