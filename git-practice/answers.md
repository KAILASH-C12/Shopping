# Git Practice – Answers to Theoretical Questions

## 17a. What a Git branch represents internally

A Git branch is simply a lightweight, movable pointer to a specific commit object in the repository's object store. Internally it is stored as a plain text file inside `.git/refs/heads/` that contains the 40-character SHA-1 hash of the commit it currently points to. When you make a new commit on a branch, Git creates a new commit object, updates that file to store the new commit's hash, and moves `HEAD` (the special pointer that tracks your current position) along with it. No actual files are copied — branches are almost free to create and switch between.

## 17b. Why `git status` is important during daily development

`git status` gives an instant snapshot of the working directory and staging area. It shows:

- **Untracked files** – new files Git has never seen before.
- **Modified files** – files that have been changed since the last commit but not yet staged.
- **Staged files** – changes that are queued to be included in the next commit.
- **Branch information** – which branch is checked out and whether it is ahead of, behind, or diverged from its remote counterpart.

Checking `git status` before every `git add` and `git commit` prevents accidentally including unwanted changes, helps catch forgotten files, and confirms the repository is in the expected state before pushing to a shared branch.

## 17c. What information `git log` provides to a developer

`git log` displays the chronological history of commits. For each commit it shows:

- **Commit SHA-1 hash** – a unique identifier for that commit.
- **Author name and email** – who made the change.
- **Date and time** – when the commit was recorded.
- **Commit message** – a human-readable description of what changed and why.

With additional flags the output can include the full diff (`-p`), a condensed one-line summary (`--oneline`), a branch graph (`--graph`), or filtering by author, date range, or file path. This information helps developers understand project history, trace when a bug was introduced, review what teammates have contributed, and audit changes before deploying.

## 17d. How Git tracks changes across branches

Git does not store file deltas per branch. Instead, every commit object stores a complete snapshot of the entire project as a tree of blob objects (file contents). When you create a branch and make a commit, Git records:

1. A **blob** for each changed file (immutable, content-addressed by SHA-1).
2. A **tree** object that maps filenames to blobs, reflecting the directory structure.
3. A **commit** object that points to that tree, lists its parent commit(s), and stores author/timestamp metadata.

When you switch branches, Git compares the trees of the two commits and updates the working directory to reflect the differences — files unique to the new branch are restored, files removed in the new branch are deleted. Because branches are just pointers to commits, and commits point to their parent(s), Git can efficiently reconstruct any historical state and compute differences between any two points in history without redundancy.
