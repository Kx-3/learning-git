# Lesson 2: Branching Strategies

## Introduction

Branching is one of the most powerful features of Git. It allows you to diverge from the main line of development and work on new features, bug fixes, or experiments in isolation without affecting the stability of the main codebase. In a collaborative environment, effective branching is crucial for managing parallel development and integrating changes safely.

This lesson will cover how Git handles branches, how to create, manage, and merge them, handle merge conflicts, and introduce common branching strategies used in software development teams.

## Understanding Branches

Conceptually, a branch in Git is simply a lightweight, movable pointer to a specific commit. When you create a new branch, Git isn't duplicating your entire project; it's just creating a new pointer to the current commit.

The default branch is usually called `main` (or `master` in older repositories). As you make commits, the branch pointer automatically moves forward to the latest commit on that branch.

### Creating Branches

You can create a new branch using the `git branch` command:

```bash
git branch feature/my-new-feature
```

This command *only* creates the new pointer (`feature/my-new-feature`). It does not automatically switch you to that branch. The new branch pointer will point to the same commit as the branch you were currently on.

### Listing Branches

To see all the branches in your local repository, use `git branch`:

```bash
git branch
```

The output will list your branches, and the current active branch will be highlighted (usually with an asterisk `*`).

To see both local and remote branches:

```bash
git branch -a
```

### Switching Between Branches

To switch from your current branch to another existing branch, use `git checkout`:

```bash
git checkout feature/my-new-feature
```

When you switch branches, Git changes the files in your working directory to match the snapshot of the commit the destination branch pointer is pointing to.

### Creating and Switching in One Command

A common operation is to create a new branch and immediately switch to it. The `git checkout -b` command does exactly this:

```bash
git checkout -b bugfix/fix-login-error
```

This is equivalent to running `git branch bugfix/fix-login-error` followed by `git checkout bugfix/fix-login-error`.

## Working with Branches

Once you've created and switched to a new branch, you can make changes and commit them as usual. These commits will only exist on your new branch, and the branch pointer will move forward with each new commit. The commits on other branches (like `main`) remain unaffected.

Let's say you are on the `feature/my-new-feature` branch:

```bash
# On feature/my-new-feature branch
echo "Adding feature code." > feature.txt
git add feature.txt
git commit -m "Implement part of new feature"
```

Now, `feature/my-new-feature` points to this new commit. If you switch back to `main` (`git checkout main`), `feature.txt` will disappear from your working directory (unless it also exists on `main` with different content), and the `main` branch pointer is still at its original position.

### Viewing Branch History

Visualizing the history with branches is very helpful. Use `git log` with graph and oneline options:

```bash
git log --oneline --graph --all
```

This command shows all commits (`--all`), displays branches and merges graphically (`--graph`), and uses a condensed one-line format (`--oneline`). You will see how different branches diverge and merge.

## Merging Branches

Once you've completed work on a branch (e.g., a feature branch or bugfix branch), you'll typically want to integrate those changes back into another branch, often the main development branch (`main`). This is done using the `git merge` command.

First, switch to the branch you want to merge *into*. For example, to merge `feature/my-new-feature` into `main`:

```bash
git checkout main
git merge feature/my-new-feature
```

Git will attempt to integrate the changes from `feature/my-new-feature` into your current branch (`main`).

### Fast-Forward Merge

If the branch you are merging *from* (`feature/my-new-feature`) is a direct descendant of the branch you are merging *into* (`main`) – meaning `main` hasn't had any new commits since you created `feature/my-new-feature` – Git can simply move the `main` pointer forward to the latest commit on `feature/my-new-feature`. This is called a **fast-forward merge**. It doesn't create a new merge commit.

```
A -- B (main)
     \
      C -- D (feature/my-new-feature)

git checkout main
git merge feature/my-new-feature

A -- B -- C -- D (main, feature/my-new-feature)
```

### Three-Way Merge

If the branch you are merging *into* (`main`) *has* had new commits since you created the other branch (`feature/my-new-feature`), Git needs to perform a **three-way merge**. It uses the common ancestor commit (the point where the branches diverged) and the latest commits on both branches to create a new snapshot. This results in a **merge commit**.

```
A -- B -- E (main)
     \
      C -- D (feature/my-new-feature)

git checkout main
git merge feature/my-new-feature

A -- B -- E -- F (main)
     \         /
      C --- D
```

Commit `F` is the new merge commit.

## Handling Merge Conflicts

Merge conflicts occur when Git tries to merge changes from two branches, and the *same parts* of the *same files* have been changed differently in both branches. Git can't automatically decide which version to keep.

When a merge conflict happens, Git will stop the merge process and tell you which files have conflicts. The `git status` command will show "Unmerged paths".

Git will modify the conflicted files in your working directory, adding special markers to indicate the conflicting sections:

```
<<<<<<< HEAD
This is the content in the current branch (main).
=======
This is the content in the branch you are merging (feature/my-new-feature).
>>>>>>> feature/my-new-feature
```

*   `<<<<<<< HEAD`: Marks the beginning of the content in your current branch (`HEAD`).
*   `=======`: Separates the content in the two branches.
*   `>>>>>>> <branch-name>`: Marks the end of the content in the branch you are merging from.

**To resolve a conflict:**

1.  Open the conflicted file(s) in your text editor.
2.  Identify the sections marked by `<<<<<<<`, `=======`, and `>>>>>>>`.
3.  Manually edit the file to keep the desired code, removing the markers. You can keep parts from both sides, one side, or write entirely new code.
4.  Save the file.
5.  Add the resolved file(s) to the staging area: `git add <conflicted-file>`. This tells Git the conflict is resolved for this file.
6.  Once all conflicts are resolved and staged, complete the merge by committing: `git commit`. Git will automatically create a merge commit (unless it was a fast-forward merge with no conflicts).

Resolving conflicts requires careful review to ensure the final code integrates the necessary changes from both branches correctly.

## Deleting Branches

Once a branch has been successfully merged and you no longer need it, you can delete it.

To delete a *local* branch:

```bash
git branch -d feature/my-new-feature
```

The `-d` flag (short for `--delete`) is a "safe" delete – Git will only delete the branch if it has been fully merged into its upstream branch (usually `main` or the branch you merged it into). If the branch contains unmerged changes, Git will warn you.

To *force* delete a local branch, even if it has unmerged changes (use with caution!):

```bash
git branch -D bugfix/work-in-progress # Use -D (capital D)
```

To delete a *remote* branch (e.g., after merging a feature branch via a Pull Request):

```bash
git push origin --delete feature/my-new-feature
# or using a shorthand syntax
git push origin :feature/my-new-feature
```

## Common Branching Strategies

Teams adopt various branching strategies to structure their development process and manage releases. Here are a few popular ones:

1.  **Feature Branching**:
    *   All development happens in dedicated feature branches (`feature/*`).
    *   Developers fork from the main branch (usually `main` or `develop`).
    *   Once a feature is complete and reviewed, it's merged back into the main branch.
    *   The main branch is always kept in a releasable state.
    *   **Pros**: Isolates feature work, simplifies collaboration, clear history.
    *   **Cons**: Can lead to integration challenges if feature branches are very long-lived.

2.  **GitFlow**:
    *   A more complex, strictly defined model using multiple long-lived branches:
        *   `main`: Production-ready code.
        *   `develop`: Latest integrated development changes.
        *   `feature/*`: New feature development (branched from `develop`, merged into `develop`).
        *   `release/*`: Preparing for a new production release (branched from `develop`, merged into `main` and `develop`).
        *   `hotfix/*`: Quickly fixing bugs in production (`main`) (branched from `main`, merged into `main` and `develop`).
    *   **Pros**: Well-defined process for managing releases.
    *   **Cons**: Can be overly complex for simpler projects or teams practicing continuous delivery.

3.  **GitHub Flow**:
    *   A simpler, lightweight model often used with continuous deployment.
    *   Only one long-lived branch: `main`.
    *   All development happens in feature branches (branched from `main`).
    *   Developers open Pull Requests early and often.
    *   After review and passing automated checks, branches are merged directly into `main`.
    *   `main` is always deployable.
    *   **Pros**: Simple, aligns well with continuous integration/deployment.
    *   **Cons**: Requires strong test automation and discipline to keep `main` stable.

Choosing the right strategy depends on the project's complexity, the team size, and the release process. GitHub Flow is popular for its simplicity and effectiveness with modern CI/CD.

## Best Practices for Branching

*   **Use Descriptive Branch Names**: Names like `feature/add-user-auth`, `bugfix/login-issue-#123`, `chore/update-dependencies`.
*   **Keep Branches Short-Lived**: Merge branches back into the main development line frequently to minimize merge conflicts and integration headaches.
*   **Merge Frequently**: Pull changes from the main branch into your feature branch regularly to keep it up-to-date.
*   **Commit Early and Often** on your feature branch.
*   **Use Pull Requests (or Merge Requests)**: For collaboration and code review before merging into important branches.

## Summary

Branching is fundamental to effective collaboration in Git. You learned:
*   Branches are pointers to commits.
*   How to create (`git branch`, `git checkout -b`), list (`git branch`), and switch (`git checkout`) branches.
*   How to merge branches (`git merge`) and the difference between fast-forward and three-way merges.
*   How to identify and resolve merge conflicts manually.
*   How to delete local (`git branch -d/-D`) and remote (`git push --delete`) branches.
*   Overview of common branching strategies (Feature, GitFlow, GitHub Flow).
*   Best practices for working with branches.

Mastering branching and merging is key to unlocking Git's collaborative potential.

## Suggested Practice

*   In a test repository, create several branches.
*   Make unique commits on each branch.
*   Practice switching between branches and observe how the working directory changes.
*   Merge one branch into another.
*   Intentionally create a merge conflict by changing the same line in the same file on two different branches, then merge and resolve the conflict.
*   Practice deleting branches after merging.
*   Try visualizing the history using `git log --oneline --graph --all` after creating and merging branches.
*   Simulate the GitHub Flow: Create a `main` branch, branch off a `feature/test` branch, make commits, and merge it back into `main`.