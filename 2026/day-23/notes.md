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

<img width="609" height="679" alt="image" src="https://github.com/user-attachments/assets/1b654910-1719-4af4-a4f3-d04df5978587" />

<hr/>

## Task 3: Push to GitHub

1. Create a new repository on GitHub (do NOT initialize it with a README)
2. Connect your local devops-git-practice repo to the GitHub remote
3. Push your main branch to GitHub
4. Push feature-1 branch to GitHub
5. Verify both branches are visible on GitHub
6. Answer in  notes: What is the difference between origin and upstream?

`In Git, origin and upstream are both names for remote repositories, but they serve different purposes in a developer's workflow, particularly when working with forks.` 

* origin - Points to our remote repository --  is the default remote repository that you cloned from (usually your own fork on GitHub).

* upstream - Points to the repository we forked from - If not forked then no need of upstream -- is a convention-based name for the original, main repository that you forked from, which you use to keep your project updated

<img width="681" height="553" alt="image" src="https://github.com/user-attachments/assets/fbfc5555-224a-4aa8-a7b1-8d3def9ff78e" />
<img width="832" height="690" alt="image" src="https://github.com/user-attachments/assets/bf7fe6dd-fa6e-485b-9397-4bf9763aa077" />

<hr/>

## Task 4: Pull from GitHub

1. Make a change to a file directly on GitHub (use the GitHub editor)
2. Pull that change to your local repo
3. Answer in your notes: What is the difference between git fetch and git pull?

`The main difference is that git fetch downloads remote changes without modifying your local working branch, while git pull downloads and automatically merges the changes into your current branch. 
The git pull command is essentially a shortcut for running git fetch followed by git merge.`

<img width="637" height="328" alt="image" src="https://github.com/user-attachments/assets/cdbe9e13-f06f-4fde-ab02-82da7e2e339b" />

<hr/>

## Task 5: Clone vs Fork

1. **Clone** any public repository from GitHub to your local machine
2. **Fork** the same repository on GitHub, then clone your fork
3. Answer in your notes:
   - What is the difference between clone and fork?
     * Clone - Clone means copying repository to your local computer. It is connected with remote. Creates a copy of a remote repository on your local machine. It is a standard Git command (git clone) used to download a project's files, history, and branches so you can work on them offline.
       
     * Fork - Copying someone else’s repository into your own GitHub account. Creates a personal copy of a repository on the hosting service's server (e.g., GitHub, GitLab) under your own account. It is a platform-specific feature, not a native Git command.

<img width="695" height="356" alt="image" src="https://github.com/user-attachments/assets/9ca29cde-4943-409c-8b3e-c6ae70865608" />

   - When would you clone vs fork?

     `When to Clone`
      
      * Direct Access: You have direct write permissions to the repository (e.g., your team's private project).
      * Local Development: You want to work on, test, or run code immediately on your computer.
      * One-time Usage: You only need a copy for a short period and do not intend to manage a separate, long-term version
  
     `When to Fork`
     
      * Contributing to Open Source: You want to contribute to a project where you lack write access.
      * Independent Development: You want to experiment or diverge from the original project without impacting it.
      * Portfolio Management: You want to keep a copy of a project on your own profile to show in your portfolio
   
   - After forking, how do you keep your fork in sync with the original repo
     
     ```To keep your fork in sync with the original repository (upstream), add the original repo as a remote, fetch its changes, and merge them into your local branch. The fastest method is using the "Sync fork" button on the GitHub repository page.```
