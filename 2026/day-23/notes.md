# Git Branching & Working with GitHub

## Task 1: Understanding Branches

1. What is a branch in Git?
  * A branch in Git is a lightweight, movable pointer to a specific commit. It represents an independent line of development, allowing developers to diverge from the main codebase to work on new features or bug fixes without affecting the stability of the main project.
  * Branches create a separate environment where changes can be tested and evaluated in isolation from other work, such as the main or master branch.
    
2. Why do we use branches instead of committing everything to main?
  * Using branches instead of committing directly to the main (or master) branch is a fundamental practice in software development that provides isolation, safety, and organization. While committing to main is possible.
  *  It creates significant risks, especially in team environments where breaking the code affects everyone.
    
3. What is HEAD in Git?
  * In Git, HEAD is a symbolic pointer or reference that indicates the current commit we have checked out in your working directory.
  * HEAD points to the snapshot of the commit that your working directory currently reflects.
  * When you create a new commit using git commit, that new commit's parent will be whatever HEAD is currently pointing to.
    
4. What happens to your files when you switch branches?
* When we switch branches in Git, our files change to match the state of the new branch. Git updates our working directory, modifying, adding, or deleting files to reflect the target branch's last commit.
* Uncommitted changes usually persist in your working directory, but if they conflict with the new branch, Git will stop the switch to protect your work.

<hr/>

## Task 2: Branching Commands — Hands-On

In your `devops-git-practice` repo, perform the following:
1. List all branches in your repo
2. Create a new branch called `feature-1`
3. Switch to `feature-1`
4. Create a new branch and switch to it in a single command — call it `feature-2`
5. Try using `git switch` to move between branches — how is it different from `git checkout`?
6. Make a commit on `feature-1` that does **not** exist on `main`
7. Switch back to `main` — verify that the commit from `feature-1` is not there
8. Delete a branch you no longer need
9. Add all branching commands to your `git-commands.md`
