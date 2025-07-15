# Lesson 1: Git Basics
## Introduction

Welcome to Week 2! This week, we dive into the world of **Git** and **Version Control**. Git is an essential tool for any software developer or DevOps engineer, allowing you to track changes to your code, collaborate with others, and manage different versions of your projects effectively.

### What is Version Control?

Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later. Think of it like "undo" for your project files, but on a much more powerful and organized level.

Why is it essential?

*   **Tracking Changes**: See who changed what, when, and why.
*   **Collaboration**: Multiple people can work on the same project simultaneously without overwriting each other's work.
*   **Backup and Restore**: Easily revert to earlier versions if something goes wrong.
*   **Branching and Merging**: Work on new features or fix bugs in isolation without affecting the main codebase.
*   **Audit Trail**: Provides a history of all project modifications.

### Centralized vs. Distributed Version Control Systems (CVCS vs. DVCS)

Historically, Version Control Systems fell into two main categories:

1.  **Centralized Version Control Systems (CVCS)**:
    *   Examples: SVN, CVS.
    *   Have a single central server that contains all the versioned files.
    *   Clients check out files from that central place.
    *   **Pros**: Easier to administer, users can see what everyone else is doing relatively easily.
    *   **Cons**: Single point of failure (if the server goes down, no one can collaborate or save versioned changes).
2.  **Distributed Version Control Systems (DVCS)**:
    *   Examples: Git, Mercurial.
    *   Clients don't just check out the latest snapshot of the files; they fully mirror the repository, including its entire history.
    *   Every clone is a full backup of the repository.
    *   **Pros**: No single point of failure, faster operations (most operations are local), allows for more flexible workflows.
    *   **Cons**: Slightly steeper initial learning curve for some concepts.

**Git** is a prime example of a **DVCS**. This distributed nature is key to its power and flexibility.

## Overview of Git

Git was created by Linus Torvalds in 2005 for the development of the Linux kernel. It's designed for speed, data integrity, and support for distributed, non-linear workflows.

*   **Speed**: Git is very fast at most operations because it operates locally.
*   **Data Integrity**: Git uses checksums (SHA-1 hashes) to ensure the integrity of its data.
*   **Distributed**: Every developer's working copy of the code is also a repository that can contain the full history of all changes.

## Installing and Configuring Git

Before you can use Git, you need to install it on your system. Installation is straightforward across different operating systems:

*   **Linux**: Usually available via package managers (e.g., `sudo apt update && sudo apt install git` on Debian/Ubuntu).
*   **macOS**: Can be installed via Homebrew (`brew install git`) or by installing the Xcode Command Line Tools.
*   **Windows**: Download the installer from the [official Git website](https://git-scm.com/downloads).

After installation, you can verify it by running:

```bash
git --version
```

Next, you should configure your user name and email address. This information is embedded in the commits you make.

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

The `--global` flag means these settings apply to all your Git repositories. You can omit `--global` to set configurations only for the current repository.

To view your current configuration, use:

```bash
git config --list
```

## Git File States: Working Directory, Staging Area, and Repository

Git has three main states your files can be in within your local environment:

1.  **Working Directory**: This is where you make changes to your files. These changes are not yet tracked by Git.
2.  **Staging Area (Index)**: This is where you prepare a snapshot of your changes before committing them to the repository. You use `git add` to add changes from the working directory to the staging area.
3.  **Git Repository (Committed State)**: This is where Git permanently stores the snapshots (commits) of your project. Changes in the staging area are moved to the repository using `git commit`.

![Git Workflow](https://git-scm.com/book/en/v2/images/areas.png)
*(Image Source: Git SCM Book)*

Your typical workflow will involve:
1.  Modifying files in your **Working Directory**.
2.  Selectively adding changes you want to include in your next commit to the **Staging Area** (`git add`).
3.  Recording the staged changes permanently into the **Git Repository** (`git commit`).

## Initializing a Repository

To start using Git for a new or existing project that isn't already under version control, navigate to the project's root directory in your terminal and run:

```bash
git init
```

This command creates a new `.git` subdirectory in your project. This directory contains all the necessary files that Git needs to track the project's history (the repository itself). You usually don't need to interact directly with the `.git` directory.

## Checking Status and Staging Changes

The `git status` command is your best friend. It shows you the state of your working directory and staging area.

Let's create a simple file:

```bash
echo "Hello, Git!" > hello.txt
git status
```

Output will show `hello.txt` as "Untracked files". Git sees the file but isn't tracking its changes yet.

To start tracking `hello.txt` and include its current state in the next commit, you add it to the staging area:

```bash
git add hello.txt
git status
```

Now, the output will show `hello.txt` as "Changes to be committed". It's in the staging area, ready for the next commit.

You can add multiple files or even all changes in the current directory and subdirectories:

```bash
git add .
# or
git add -A # Stages all changes, including deletions
```

## Committing Changes

Once your desired changes are in the staging area, you commit them to the repository using `git commit`. A commit is a snapshot of your staged changes at a specific point in time. Each commit has a unique identifier (a SHA-1 hash) and includes the author, timestamp, and a commit message.

```bash
git commit -m "Initial commit: Add hello.txt"
```

The `-m` flag allows you to provide a short commit message directly in the command. It's crucial to write clear and descriptive commit messages so you and others understand what changes were made in that commit.

If you omit `-m`, Git will open your configured text editor, allowing you to write a more detailed multi-line commit message.

After committing, `git status` should show a clean working directory, indicating no pending changes.

## Viewing Commit History

To see the history of commits in your repository, use the `git log` command:

```bash
git log
```

This will show commits in reverse chronological order, including the author, date, and commit message.

Useful variations of `git log`:

*   `git log --oneline`: Shows a condensed view of the history.
*   `git log --graph`: Visualizes the branch and merge history (more useful when you start branching).

## Making More Changes and Committing Again

Let's modify `hello.txt` and create another commit:

```bash
echo "Adding another line." >> hello.txt
git status
```

`git status` will show `hello.txt` as "Changes not staged for commit". It's modified in the working directory.

```bash
git add hello.txt # Add the modified file to the staging area
git status
```

Now it's "Changes to be committed".

```bash
git commit -m "Add a second line to hello.txt"
git log --oneline
```

You should now see two commits in the history.

## Ignoring Files

Sometimes, you have files in your project directory that you don't want Git to track or include in commits. This could be compiled code, build artifacts, dependency directories (like `node_modules/`), or sensitive configuration files. You can tell Git to ignore these files by creating a `.gitignore` file in the root of your repository.

Create a `.gitignore` file:

```bash
touch .gitignore
```

Edit `.gitignore` and add patterns for files/directories to ignore. Each line is a pattern.

```
# Ignore compiled Python files
*.pyc

# Ignore the node_modules directory
node_modules/

# Ignore a specific file
my_config.txt

# Ignore all files ending with ~
*~

# Ignore a directory and everything inside it
temp/

# Ignore a file named 'build.log' in the root directory
/build.log
```

After saving `.gitignore`, Git will stop showing files matching these patterns as "Untracked". You should add and commit your `.gitignore` file itself so that the ignore rules are shared with anyone cloning the repository.

```bash
git add .gitignore
git commit -m "Add .gitignore file"
```

## Connecting to a Remote Repository (GitHub Example)

Git's power is fully realized when collaborating using remote repositories. GitHub is a popular platform for hosting Git repositories.

Assuming you have a project initialized locally with Git and a corresponding empty repository created on GitHub (or another platform like Azure Repos, GitLab, Bitbucket, etc.):

1.  **Add a remote origin**: This tells your local repository where the remote repository is located. Replace `<remote-url>` with the URL provided by GitHub (usually HTTPS or SSH).

    ```bash
    git remote add origin <remote-url>
    ```

    `origin` is just a conventional name for the primary remote repository.

2.  **Push local commits to the remote**: Send your committed changes from your local repository to the remote. The `-u` flag sets the upstream branch, so subsequent pushes/pulls can be done with just `git push` or `git pull`. `main` is the default branch name in most new repositories.

    ```bash
    git push -u origin main
    ```

    This will push the `main` branch from your local repository to the `origin` remote.

3.  **Cloning a remote repository**: If a project already exists on a remote repository, you can get a full local copy (including all history, branches, etc.) by cloning it:

    ```bash
    git clone <remote-url>
    ```

    This command initializes a local Git repository, adds the remote (named `origin`), fetches all data, and checks out the default branch (usually `main`).

## Pulling Changes from a Remote Repository

When others are working on the same project, they will push their changes to the remote repository. You need to pull those changes to update your local copy.

The `git pull` command is a shortcut for two operations:

1.  **Fetch**: Downloads commits, files, and refs from the remote repository into your local repository, but *does not* merge them with your current branch.

    ```bash
    git fetch origin
    ```

    After fetching, you can see what has changed on the remote compared to your local branches.

2.  **Merge**: Integrates the fetched changes into your current local branch.

    ```bash
    git merge origin/main # Merges the remote main branch into your current local branch
    ```

**`git pull`** combines these two steps:

```bash
git pull origin main
```

This fetches changes from the `main` branch of the `origin` remote and automatically merges them into your current local branch.

## Summary

In this lesson, you learned the absolute fundamentals of Git:
*   What version control is and why Git is a powerful DVCS.
*   The core Git workflow (Working Directory, Staging Area, Repository).
*   Initializing a repository (`git init`).
*   Checking status (`git status`).
*   Staging changes (`git add`).
*   Committing changes (`git commit`).
*   Viewing history (`git log`).
*   Ignoring files (`.gitignore`).
*   Connecting to a remote repository (`git remote add`, `git push`, `git clone`).
*   Updating your local copy from a remote (`git fetch`, `git merge`, `git pull`).

These commands form the basis of your daily interaction with Git.

## Suggested Practice

*   Initialize a new Git repository for a small personal project or simply a test directory.
*   Create several files, add them, and make multiple commits.
*   Modify files and commit the changes.
*   Experiment with `git status` and `git log`.
*   Create a `.gitignore` file and add patterns for files you don't want to track.
*   Create a corresponding empty repository on GitHub and push your local repository to it.
*   Clone the repository to a *different* directory on your computer to simulate another user cloning it.
*   Make changes in the cloned repository and push them.
*   Go back to your original repository and pull the changes.

edit