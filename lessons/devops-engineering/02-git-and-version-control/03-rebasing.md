# Lesson 3: Advanced Git: Rebasing, Merging and Resolving conflicts
## Introduction

In the previous lesson, we explored the fundamentals of branching and merging. Merging is a primary way to integrate changes from one branch into another. However, Git offers another powerful technique for integrating changes: **rebasing**. While both achieve the goal of combining work, they do so differently and result in different commit histories. Understanding when and how to use rebasing, along with advanced conflict resolution and other useful Git commands, is crucial for maintaining a clean and understandable project history.

## Rebasing

Rebasing is the process of moving or combining a sequence of commits to a new base commit. It essentially rewrites commit history.

Consider this history:

```
A --- B --- C (main)
     \
      D --- E --- F (feature)
```

If you are on the `feature` branch and want to rebase it onto `main`:

```bash
git checkout feature
git rebase main
```

Git will:
1.  Find the common ancestor of `main` and `feature` (commit `B`).
2.  Get the diff introduced by each commit on the `feature` branch since the common ancestor (commits `D`, `E`, `F`).
3.  Save those diffs temporarily.
4.  Reset the `feature` branch pointer to the tip of the `main` branch (commit `C`).
5.  Apply each saved diff onto the new base (`C`) in order. This creates *new commits* (`D'`, `E'`, `F'`).

The resulting history looks like this:

```
A --- B --- C --- D' --- E' --- F' (main, feature)
```

Now, the `feature` branch appears as if it was created directly from the latest `main` commit (`C`). The history is linear and often considered cleaner.

### When to Use Rebasing vs. Merging

*   **Use Merge when:**
    *   You want to preserve the exact history of your branches, including merge commits.
    *   You are integrating changes into a public or shared branch (like `main` or `develop`) that others have already pulled. **Rule of thumb: Never rebase commits that others have based their work on, as it can cause significant issues when collaborating.**
*   **Use Rebase when:**
    *   You are working on a local, private feature branch.
    *   You want to have a clean, linear commit history, avoiding extraneous merge commits.
    *   You want to incorporate the latest changes from `main` into your feature branch without creating a merge commit.

**Example Scenario:** You're working on `feature` branch, and `main` has progressed. To keep your `feature` branch up-to-date before merging it back into `main`, you can rebase `feature` onto `main`:

```bash
git checkout feature
git rebase main
# Resolve any conflicts that arise during the rebase
```

This brings your `feature` branch commits on top of the latest `main` commits, making the eventual merge back into `main` a simple fast-forward (if no new commits were added to `main` in the meantime).

### Interactive Rebase (`git rebase -i`)

Interactive rebase is a powerful tool for rewriting history even further on a local branch. It allows you to:

*   **Pick**: Keep a commit as is.
*   **Reword**: Change the commit message.
*   **Squash**: Combine a commit with the previous one.
*   **Fixup**: Squash a commit with the previous one, discarding the commit message.
*   **Edit**: Stop the rebase process after a specific commit to amend it or perform other actions.
*   **Drop**: Remove a commit entirely.

To start an interactive rebase, specify the commit *before* the sequence you want to modify. For example, to rebase interactively the last 3 commits on your current branch:

```bash
git rebase -i HEAD~3
```

Git will open your editor with a list of the commits and instructions. You modify the list (e.g., changing `pick` to `squash`) and save. Git then performs the actions you specified.

**Use interactive rebase cautiously and only on private branches!**

## Rebasing Conflicts

Just like merging, rebasing can lead to conflicts. When Git encounters a conflict during a rebase, it stops the process and tells you which files are conflicted.

The process to resolve rebase conflicts is similar to merge conflicts:

1.  Edit the conflicted file(s) and manually resolve the conflicts (markers `<<<<<<<`, `=======`, `>>>>>>>` will show the conflicting sections).
2.  Save the file(s).
3.  Stage the resolved file(s): `git add <conflicted-file>`. **Do NOT commit at this stage!**
4.  Continue the rebase: `git rebase --continue`.

If you decide you don't want to continue the rebase, you can abort it: `git rebase --abort`.

## Resolving Conflicts (Advanced)

While manual editing with markers is the standard way, Git provides tools to help:

*   **Merge Tools**: You can configure external merge tools and use `git mergetool` to launch them to guide you through conflict resolution visually.

    ```bash
    git mergetool
    ```

    This is often easier for complex conflicts across multiple files.

## Comparing Changes

The `git diff` command is essential for seeing exactly what has changed between different points.

*   **Working Directory vs. Staging Area**: Shows changes in the working directory that are *not* in the staging area.

    ```bash
    git diff
    ```

*   **Staging Area vs. Last Commit**: Shows changes in the staging area that are *not* in the last commit (`HEAD`).

    ```bash
    git diff --staged # or git diff --cached
    ```

*   **Working Directory vs. Last Commit**: Shows all changes since the last commit.

    ```bash
    git diff HEAD
    ```

*   **Between two commits/branches**: Shows the differences between the tips of two branches or between two specific commits.

    ```bash
    git diff main..feature # Changes in feature that are not in main
    git diff <commit1-hash>..<commit2-hash>
    ```

## Undoing Changes

Making mistakes is part of development. Git provides several ways to undo changes, depending on what you want to achieve and where the changes are (working directory, staging, or committed).

*   **Unstaging a File**: Move a file from the staging area back to the working directory. (Use `git restore` in newer Git versions, or `git reset HEAD <file>` in older versions).

    ```bash
    git restore --staged my_file.txt # Removes from staging
    ```

*   **Discarding Changes in Working Directory**: Revert changes in the working directory to match the state in the staging area or the last commit.

    ```bash
    git restore my_file.txt # Discards changes in working directory
    git restore . # Discards all changes in working directory
    ```

*   **Amending the Last Commit**: Add newly staged changes to the *most recent* commit instead of creating a new one. You can also use it to just change the last commit message.

    ```bash
    # Make changes, git add them
    git commit --amend --no-edit # Add staged changes to last commit, keep same message
    git commit --amend # Add staged changes and edit the message
    ```

*   **Resetting to a Previous Commit**: Move the current branch pointer to a previous commit. This effectively removes later commits from the branch's history.
    *   `--soft`: Moves the branch pointer, but keeps the changes in the staging area.
    *   `--mixed` (default): Moves the branch pointer, moves changes to the working directory (unstaged).
    *   `--hard`: Moves the branch pointer and discards all changes (in staging and working directory). **Use `--hard` with extreme caution, as it can lose work!**

    ```bash
    git reset HEAD~1 --soft # Move pointer back 1 commit, keep changes staged
    git reset HEAD~1 # Move pointer back 1, changes unstaged in working dir
    git reset HEAD~1 --hard # Move pointer back 1, discard all changes
    git reset <commit-hash> --hard # Move pointer to a specific commit, discard changes
    ```

    **Important**: Do not `git reset --hard` commits that you have already pushed to a shared remote repository, as this will cause problems for collaborators.

*   **Reverting a Commit**: Create a *new commit* that undoes the changes introduced by a previous commit. This is a safe way to undo committed changes because it doesn't rewrite history.

    ```bash
    git revert <commit-hash>
    ```

    Git will create a new commit with the reversed changes and prompt you for a commit message.

## Stashing Changes

Sometimes you're working on a branch, but need to temporarily switch to another branch to fix something quickly. However, you have uncommitted changes in your working directory and staging area. You don't want to commit them yet because the work isn't finished. `git stash` saves your local modifications away and reverts the working directory to match the `HEAD` commit.

*   **Save changes**:

    ```bash
    git stash save "Work in progress for feature X" # Optional message
    # or simply
    git stash
    ```

    Your working directory is now clean.

*   **List stashes**: See the stashes you've saved.

    ```bash
    git stash list
    ```

*   **Apply a stash**: Reapply the stashed changes. By default, applies the most recent one (`stash@{0}`).

    ```bash
    git stash apply # Applies most recent stash
    git stash apply stash@{2} # Applies a specific stash
    ```

    `apply` keeps the stash in the list.

*   **Apply and drop a stash**: Apply the most recent stash and then remove it from the list.

    ```bash
    git stash pop
    ```

*   **Drop a specific stash**: Remove a stash without applying it.

    ```bash
    git stash drop stash@{1}
    ```

*   **Clear all stashes**: Remove all stashes.

    ```bash
    git stash clear
    ```

## Working with Tags

Tags are pointers to specific commits, typically used to mark release points (e.g., v1.0, v2.0-beta). Unlike branch pointers, tags don't move.

*   **Create a lightweight tag**: A simple pointer to a commit.

    ```bash
    git tag v1.0 # Tags the current HEAD commit
    git tag v1.0 <commit-hash> # Tags a specific commit
    ```

*   **Create an annotated tag**: More robust, stores the tagger's name, email, date, and a tagging message. Recommended for releases.

    ```bash
    git tag -a v2.0 -m "Release version 2.0" # Tags the current HEAD commit
    git tag -a v2.0 <commit-hash> -m "Release version 2.0" # Tags a specific commit
    ```

*   **List tags**:

    ```bash
    git tag
    git tag -l "v1.*" # Filter tags
    ```

*   **Show tag details**:

    ```bash
    git show v2.0 # Shows details of an annotated tag
    ```

*   **Push tags to remote**: Tags are not pushed by default.

    ```bash
    git push origin v1.0 # Push a specific tag
    git push origin --tags # Push all local tags
    ```

*   **Delete a local tag**:

    ```bash
    git tag -d v1.0
    ```

*   **Delete a remote tag**:

    ```bash
    git push origin --delete v1.0 # Or git push origin :v1.0
    ```

## Summary

This lesson introduced advanced Git concepts:
*   **Rebasing** (`git rebase`) as an alternative to merging for creating clean, linear history (use on private branches).
*   Handling conflicts during **rebasing**.
*   Comparing changes with `git diff`.
*   Various methods for **undoing changes** (`git restore`, `git commit --amend`, `git reset`, `git revert`) and understanding when to use each.
*   Temporarily saving changes with **stashing** (`git stash`).
*   Using **tags** (`git tag`) to mark important commits like releases.

Understanding these advanced tools gives you more control over your project's history and workflow. Always be mindful of rewriting history, especially on shared branches.

## Suggested Practice

*   In a test repository, create a `main` branch and a `feature` branch.
*   Make a few commits on `main`.
*   Make a few commits on `feature`.
*   Make another commit on `main`.
*   Try merging `feature` into `main`. Observe the merge commit.
*   Now, reset `main` back before the merge (use `git reflog` to find the commit hash you want to reset to, then `git reset --hard <commit-hash>`).
*   Switch back to `feature`.
*   Try rebasing `feature` onto `main`. Observe the linear history (you might need to force push if you push this history to a remote for practice, but be *very* careful doing this on real shared repos).
*   Create a conflict scenario on two branches and practice resolving it manually.
*   Make some changes, stage some, leave some unstaged, then use `git stash`. Practice applying and popping stashes.
*   Create a lightweight tag and an annotated tag.
*   View the history (`git log --oneline --graph --all`) and identify the commits pointed to by your branches and tags.