# Introduction to Git: Your First Repository

## Task 1: Install and Configure Git

* Verify Git is installed on your machine
* Set up your Git identity — name and email
* Verify your configuration

<img width="560" height="442" alt="image" src="https://github.com/user-attachments/assets/d79b2934-2386-42bf-b952-528c001e51d0" />

<hr/>

## Task 2: Create Your Git Project

* Create a new folder called devops-git-practice
* Initialize it as a Git repository
* Check the status — read and understand what Git is telling you
* Explore the hidden .git/ directory — look at what's inside

<img width="599" height="399" alt="image" src="https://github.com/user-attachments/assets/3b673649-57b1-4c68-a9cc-075955d83d03" />

<hr/>

## Task 3: Create Your Git Commands Reference

1. Create a file called `git-commands.md` inside the repo
2. Add the Git commands you've used so far, organized by category:
   - **Setup & Config**
   - **Basic Workflow**
   - **Viewing Changes**
3. For each command, write:
   - What it does (1 line)
   - An example of how to use it

Soultion - check `git-command.md`

<hr/>

## Task 4: Stage and Commit

1. Stage your file
2. Check what's staged
3. Commit with a meaningful message
4. View your commit history

<img width="678" height="524" alt="image" src="https://github.com/user-attachments/assets/8ceab2c6-b8ac-4c8d-a007-6379d8b2e314" />

<hr/>

## Task 5: Make More Changes and Build History
1. Edit `git-commands.md` — add more commands as you discover them
2. Check what changed since your last commit
3. Stage and commit again with a different, descriptive message
4. Repeat this process at least **3 times** so you have multiple commits in your history
5. View the full history in a compact format

<img width="402" height="48" alt="image" src="https://github.com/user-attachments/assets/421ae7df-9d4b-4add-95d7-c0f7796446a8" />

<img width="590" height="659" alt="image" src="https://github.com/user-attachments/assets/c3bc8138-6905-4ef3-8c90-f8fe0ae57570" />

<hr/>

1. What is the difference between `git add` and `git commit`?

   `git add`

      * Function: git add moves changes from your working directory (where you edit files) to the staging area (also known as the index).
   
      * Purpose: The staging area acts as a buffer, allowing you to review and select exactly which modifications, deletions, and new files you want to include in the next single commit. This allows you to create focused, logical commits, even if you have made many unrelated changes in your working directory.
   
      * Effect: It prepares the content for the next commit but does not save the changes permanently in the repository's history.

      * Usage: You can use git add <filename> for specific files or git add . to stage all changes in the current directory

   `git commit`

      * Function: git commit takes everything from the staging area and stores a permanent snapshot of that exact state in the local Git repository's history.

      * Purpose: It creates a "save point" in your project's timeline that you can refer back to later. Each commit is associated with a unique identifier (a SHA-1 hash) and a descriptive message that explains what changes were made and why.

      * Effect: The changes are permanently recorded in your local repository. They are not yet shared with remote repositories (like GitHub) until you use git push.

      * Usage: The command git commit -m "Your descriptive message" is commonly used to create a commit with an inline message.

2. What does the staging area do? Why doesn't Git just commit directly?

   The Git staging area (also known as the "index") is an intermediate layer where you can curate and review changes before you record them in your project history. Git doesn't commit changes directly from the working directory to give you precise control over exactly what goes into each snapshot.

   If Git committed directly from the working directory, you would be forced to commit every single change you've made since the last commit, even unrelated ones. 

3. What information does git log show you?

   `git log` - shows commit history.

   The git log command displays the committed snapshots of a project, in reverse chronological order (latest first), by showing the following information for each commit: 

   * Commit ID: A unique 40-character SHA-1 checksum (hash) that identifies the specific commit.
   * Author: The name and email address of the person who created the commit.
   * Date and Time: A timestamp indicating when the commit was made.
   * Commit Message: A description of the changes made, typically including a short title and a body.

4. What is the .git/ folder and what happens if you delete it?

   The .git/ folder is the core repository and database for your project's entire version history. Deleting it will effectively erase all local Git tracking data, turning your project folder into a regular directory of files that are no longer under version control.

`What is the .git/ folder?`

The .git/ folder (which is hidden by default) contains all the necessary information for Git to manage your project. It is essentially Git's "brain" or internal database, created when you run git init or git clone. 

Key contents include:
   *objects/: The primary database where all your files (as "blobs"), directory structures (as "trees"), and commits are stored as compressed objects with unique hash identifiers.
   
   * refs/: Contains pointers (references) to commits, including the location of different branches (refs/heads) and tags (refs/tags).
   
   * HEAD: A file that points to the current branch you are working on, indicating the latest checked-out revision.
   
   * index: The staging area (or cache file) that stores a snapshot of the changes ready to be committed.
   
   * config: The local configuration file specific to your repository (e.g., remote URLs, user information).
   
   * hooks/: Scripts that can be triggered automatically before or after certain Git events, such as committing or pushing. 

`What happens if you delete it?`

   * Deleting the .git/ folder has significant consequences, primarily:
   
   * Loss of Version Control: Your project will no longer be a Git repository. Running any git command will result in an error message stating that it's "not a git repository".
   
   * Irreversible History Loss (Locally): All your local commit history, branches, and tags are stored exclusively in this folder and will be lost. You will no longer be able to revert to previous versions of files or view the project's evolution over time.
   
   * Project Files Remain Untouched: The actual source code and other project files in your working directory will remain on your disk, but they will be untracked by Git.
   
   * Loss of Remote Connection: The link to any remote repository (e.g., on GitHub or GitLab) is stored in the .git/config file, so your local project will lose this connection.

5. What is the difference between a working directory, staging area, and repository?

   * Working Directory (Working Tree/Area): The actual folder on your computer where you are currently modifying, creating, or deleting files. Files here are often "untracked" until added to the staging area.

   * Staging Area (Index/Cache): An intermediate zone between the working directory and the repository. You use git add to move changes here, which marks them to be included in the next commit. It allows you to selectively choose changes rather than committing everything at once.

   * Repository (.git Directory): The hidden folder (.git) where Git stores the metadata and object database for your project. This is the permanent database that holds the history and snapshots of your project, created by running git commit
  
