# Version Control with Git

`Version Control Systems(VCS)` are software tools that help software teams manage changes to source code over time. <br>
VCS allows programmers keep track of every modification to the code in a special kind of database, and turn back the clock and compare earlier versions of the code to help fix the mistake

`Git` is a distributed VCS, designed to track changes in source code, enabling multiple developers to work together on non-linear development.

- **Distributed Architecture**: Every developer's working copy of the code is also a repository that can contain the full history of all changes.
- **Branching and Merging**: Git offers excellent tools to branch and merge, making it possible for developers to have multiple local branches that can be entirely independent of each other.

`GitHub` is a cloud-based hosting service that lets you manage Git repositories.

- Pull Requests: An essential feature for team collaborations, which allows to notify others about changes you've pushed to a branch in a repository on GitHub.


## Getting Started

1. Cloning a Repository (like GitHub)

   ```shell
   git clone URL
   ```

2. Initializing a New Repository

   ```shell
   git init
   git remote add origin REMOTE_URL
   # `origin` is a conventional name for your primary remote repository
   # Replace <REMOTE-URL> with the URL you copied from GitHub

   # To ensure that your local repository is connected to the remote repository, run
   git remote -v
   # which will list the remote connections for your repository
   ```

**Cloning** copies a repository AND creates corresponding working files.

**`.git`** subdirectory in the project, which makes the project directory a "repository". This directory is hidden by default.

**NOTE:** We don't necessarily have a "boss and servant" relationship between upstream and downstream repositories. Often times, a clone can become more active/popular than the original, in which case, the latter will start to sync with the former instead of vice versa.

## Git Workflows

Git used `workflows` which can be broken into three steps and three states namely

```
+-------------------+       +--------------+       +-----------------+       +-------------------+
| Working Directory |       | Staging Area |       | Committed Files |       | Remote Repository |
+-------------------+       +--------------+       +-----------------+       +-------------------+

                     git add               git commit                 git push
                   ----------->           ----------->              ----------->
```

### `Modified`

**Modify files** in your working directory. <br>
Adding, Removing, and Updating any files inside the repository all belong to Modified state. In this state, Git knows the file has changed, but does not track it.

- `$ git add file_name` - used to stage the file into commit (adds changes to the staging area)
- `$ git restore --stage file_name` - unstage the file from the commit

### `Staged`

**Stage the changes**, marking them for inclusion in the next commit <br>
In order for Git to track a file, it needs to put(add) in the staged area. Once added, any modifications are tracked.

- `$ git commit` - commit the file inside the staged area (records the staged changes in the version history.)
  - `-m "MESSAGE"` (to attach a message about this commit)

### `Committed`

**Commit the changes**, which takes the files as they are in the staging area and stores that snapshot permanently in your Git directory. <br>
Committing a file in Git is like a save point in many ways. Git will save the file, and have a snapshot of the current changes.

- `$ git push` - publish the local commits to a remote repository


## Exploring the Log

Display information about every previous commit leading up to the current version, in reverse order by default:

```shell
git log
# displays the commit history of the entire repository

# the following is an output example
commit 9a2d3b4c5f6e7d8c9f0a1b2c3d4e5f6789012345 # Commit ID
Author: Jane Doe <jane.doe@example.com>
Date:   Wed Feb 15 14:03:12 2024 -0500

    Add login feature
```

Each commit has a unique `Commit ID`, which is a 40-character hash string representing the commit's contents.

Other Common Commands

```shell
git log --oneline
# Condenses each commit to a single line, showing a shortened commit ID and the commit message

git log --stat
# Provides a summary of the changes included in each commit, showing the number of additions and deletions in files

git log --author="Author_Name"
# Filters the log to show only commits made by a specific author

git log --pretty=format:"%h - %an, %ar: %s"
# Customizes the output format to show the short hash (%h), author name (%an), author date relative (%ar), and the commit message (%s)

git log --oneline --decorate --graph --all
# Display the commit history as a decorated graph, including all branches (each commit to a single line)

git log -S"function_name"
# Searches the commit history for commits that introduce or remove an occurrence of such specific string
```

Usage of `--`

```shell
git log -- file_name
git log -- PATH
# `--` is used to tell Git that what follows is a path or file name, not another option or a branch name

git log --oneline
# if the following is an option, it should connect to `--` without extra space
```

### Formatting the Log

```shell
git log --pretty=fuller
```

The fuller format shows that every commit has an **author** AND a **committer**, each with their own dates. Git distinguishes between these two contributors. This separation is routine for many larger projects. The author would be the person that writes the code, and the committer would be the overseer that reviews the and confirms the changes. These fields _establish responsibility_ for changes, a major reason for using version control in the first place.

One interesting thing you might notice about repositories that have been in development for a long time: you might notice that commit timestamps go back even before Git existed. This is because the project may have migrated from other version control systems, such as RVS and CVS, and the date data is preserved when the history is copied.

### Narrowing the Log

Narrowing the log in Git is to filter the commit history for more targeted information(or range).

You can use the special `..` syntax to specify a commit range. The following displays the history between commits with ref `A` (exclusive) and `B` (inclusive), so like `(A, B]`. This is like "show me everything that led up to B, but exclude everything that led up to A":

```shell
git log A..B
```

Shows all commits from the very beginning of the project up to and including B

```shell
git log B
```

ps. Both A and B are Commit ID.

You can also use **"version arithmetic"** to get references to commits based on aliases, like branch names, tag names, `HEAD`, etc. [More on commit notation in Git Internals](Git%20Internals.md). The official documentation is also nice: https://git-scm.com/docs/gitrevisions.

**`HEAD`** is a special pointer refers to the current commit your working directory is on. It's a moving pointer that changes as you switch branches or make new commits.

**`^`** is Parent References syntax specifies the parent of a reference, so `HEAD^` means the commit just before `HEAD`, `HEAD^^` means the grandparent commit, etc.

Example about showing the most recent commit:

```shell
git log HEAD^..HEAD
# Shows the commit just before the current one:

git log HEAD^!
# Shorthand achieves the same result
```

## Configuring Git

Git configuration is handled through various files, including .gitignore, .git/config, and ~/.gitconfig, each serving different purposes.

### `.gitignore` File

.gitignore file specifies intentionally untracked files that Git should ignore. <br>
Files listed in .gitignore won't show up in untracked files lists and won't be added to your repository.

.gitignore is like a configuration file that instructs how users run Git. It's under Git's control i.e. it'll show up in `git ls-files`.

**What files should be ignored?**

Files that we do not want to put under version control. Obvious candidates include:

- Temporary files, such as log files, cache directories (_.log, _.cache) `\#*`
- Machine-Dependent Code, such as compiled binaries or object files (_.o, _.exe)
- Imported files (external packages or libraries that should not be part of your source control)
- Sensitive Information
  - Authentication credentials (passwords/keys/etc.)
  - Hashes of passwords? If it's intended for authentication, this would be just as bad as raw passwords, so ignore them too. Hashes enable **rainbow attacks** on the passwords where attackers try to crack the checksum algorithm.

### `.git/config` File

.git/config is located in each Git repository and contains configuration options specific to that single repository. These options include remote repository addresses, branches configuration, and more.

You can view the current configuration in `.git/config` with:

```shell
git config -l
```

This outputs the information stored in the editable `.git/config` file, which is specific to the current repository. Cloning a repository also copies the configuration file.

**CAUTION:** Be careful when editing this file, syntax errors can cause Git to malfunction and stop working.

`.git/config` is NOT under version control because it determines how Git itself functions and because it would introduce the problem of recursion. `.gitignore` IS under version control because it's like a message from the developer and contains information about how to manage the project actually being version controlled. You also don't need to worry about what's in `.gitignore` to use Git itself.

### `~/.gitconfig` File

For **Global Git Configuration** (like name, email address, preferred text editor, etc.) that applies to all of your projects, settings are stored in the ~/.gitconfig file. <br>
These settings can be added or modified either by directly editing the file or using the following command:

```shell
git config --global KEY VALUE
```

\*\*Set up global username and email:\*\*

```shell
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

You can also use the global .gitconfig file to set preferences that make your workflow smoother and more efficient across all your projects. <br> Other cool things you can specify in this file:

You can edit directly inside the file like:

```ini
[core]
    # If you don't like being dropped into Vim by default,
    # This sets it to VS Code (you can also use Emacs, etc.)
    editor = code

[alias]
    # Abbreviations
    s = status
    co = checkout
    cm = commit -m
    # etc.
```

Or use `git config --global` command:

```shell
# Specify default editor
git config --global core.editor "code --wait"

# Define useful aliases
git config --global alias.s status
git config --global alias.co checkout
git config --global alias.cm "commit -m"
```

## Branching with Git

Branching allows multiple development lines to proceed in parallel. This feature is a cornerstone of collaborative work, enabling different features or fixes to be developed simultaneously without interfering with each other.

### Branch Operations

**1. Creating a Branch**

`$ git checkout -b branch_name` - creates a new branch and switches to it

- `-b` - stands for "branch", indicating to create a new branch

`$ git branch branch_name` - creates a new branch if it does not already exist, but will not switch to it.

**2. Switching a Branch**

`$ git checkout branch_name` - switches from your current branch to another existing branch

- `-f` - stands for "force", proceed with the checkout even if there are potential conflicts or changes

**3. Listing All Branches**

`$ git branch` - shows all local branches

- `-a` will show both local and remote branches

**4. Deleting a Branch**

`$ git branch -d branch_name` - deletes the specified branch

### Examples and Use Cases

**1. Feature Development** <br>
When starting work on a new feature, create a new branch off the main branch using `$ git checkout -b feature/new-feature`. This isolates your work from the main codebase until it's ready to be merged back.

`feature/new-feature` is the name of the branch you are creating. Using a prefix of `feature/` is a convention that helps in organizing branches. It is not mandatory but recommended in many workflows for clarity.

**2. Bug Fixes** <br>
Similarly, for bug fixes, you might create a branch like `$ git checkout -b fix/login-bug`. This allows for targeted work on fixing the issue without affecting ongoing development in other areas.

### Merging Branches

Once your work on a branch is complete and tested, you'll want to merge it back into your main branch (or another parent branch). git merge branch_name merges the specified branch into your current branch, combining the histories of the two branches.

Handling Merge Conflicts
Merge conflicts occur when Git can't automatically resolve differences in code between two commits. When this happens, Git pauses the merge and asks you to resolve the conflicts manually. After resolving the conflicts, you can continue the merge process.
