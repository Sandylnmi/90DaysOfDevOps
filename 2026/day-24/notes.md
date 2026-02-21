# Advanced Git: Merge, Rebase, Stash & Cherry Pick

### Task 1: Git Merge — Hands-On
1. Create a new branch `feature-login` from `main`, add a couple of commits to it
2. Switch back to `main` and merge `feature-login` into `main`
3. Observe the merge — did Git do a **fast-forward** merge or a **merge commit**?

<img width="755" height="536" alt="image" src="https://github.com/user-attachments/assets/84085f75-cd73-4634-b160-f5f507252947" />

4. Now create another branch `feature-signup`, add commits to it — but also add a commit to `main` before merging
5. Merge `feature-signup` into `main` — what happens this time?

<img width="784" height="334" alt="image" src="https://github.com/user-attachments/assets/d45c765a-dea6-4f92-b485-c9dbfddf27dd" />
<img width="766" height="616" alt="image" src="https://github.com/user-attachments/assets/541fbabb-7710-42d6-9ba5-4c0a3cc99217" />

6. Answer in your notes:
   - What is a fast-forward merge?

     * A fast-forward merge in Git is a type of merge that occurs when the target branch (e.g., main) has not diverged from the source branch (e.g., feature), allowing Git to simply move the branch pointer forward to the latest commit without creating a new merge commit.

      * It merges two branches without creating a new commit. Instead, it simply moves the current branch’s pointer forward to match the branch being merged.
        
   - When does Git create a merge commit instead?
      
      * Git creates a merge commit when the histories of the two branches being merged have diverged and a fast-forward is not possible, or when explicitly requested
      * When we try to merge two branches that have diverged. Git makes a new commit to combine the incoming changes.
        
   - What is a merge conflict? (try creating one intentionally by editing the same line in both branches).
     
      * A merge conflict is a Git event that occurs when competing changes are made to the same line(s) of a file in different branches, or when one person edits a file that another person deleted. Because Git cannot automatically determine which version to keep, it pauses the merge, highlights the conflicting sections, and requires manual intervention to resolve the issue before continuing.
    
      * A merge conflict occurs when the same part of a file is changed in two branches. When merging, Git cannot automatically decide which change to keep. Git will pause and require you to resolve the conflict before completing the merge.

<hr/>

### Task 2: Git Rebase — Hands-On
1. Create a branch `feature-dashboard` from `main`, add 2-3 commits
2. While on `main`, add a new commit (so `main` moves ahead)
3. Switch to `feature-dashboard` and rebase it onto `main`

<img width="793" height="701" alt="image" src="https://github.com/user-attachments/assets/04171440-f64a-4bac-a322-b0397459fcac" />
<img width="765" height="544" alt="image" src="https://github.com/user-attachments/assets/bd23f12c-a033-47f0-8e5f-b827959fe4fe" />

4. Observe your `git log --oneline --graph --all` — how does the history look compared to a merge?

<img width="690" height="191" alt="image" src="https://github.com/user-attachments/assets/c7156d86-a984-42de-9004-fc686733c553" />

5. Answer in your notes:
   - What does rebase actually do to your commits?- Rebase creates linear history.
      
      * Rebase in Git moves a sequence of commits to a new base commit, effectively rewriting project history by creating entirely new commits for each original one.
      
      * It replays your branch's changes on top of another branch (e.g., main), creating a linear, clean history without merge commits.
        
   - How is the history different from a merge?

      * Rebase: Use for local, private feature branches to clean up history before merging.
        <img width="427" height="240" alt="image" src="https://github.com/user-attachments/assets/71df62b1-65b4-4e1d-8992-1d7606f5a148" />

      * Merge: Use for public, shared branches to accurately record when code was integrated.
        <img width="439" height="222" alt="image" src="https://github.com/user-attachments/assets/5bdff410-9cef-4c27-9594-72b52b64dd7a" />

   - Why should you **never rebase commits that have been pushed and shared** with others?

      * Rebasing commits that have been pushed and shared with others is strongly discouraged because it rewrites the project history, leading to significant complications, conflicts, and potential data loss for all collaborators.
    
      * We should never rebase commits that have been pushed and shared with others because rebasing rewrites the project history by creating new commits with different IDs.
      
      * If other developers have based their work on the original commits, their local histories will diverge from the rebased remote history, leading to confusion, difficult-to-resolve conflicts, and potentially lost work
    
    - When would you use rebase vs merge?
      `Use git rebase to keep a clean, linear history by moving feature branches onto the tip of the main branch before sharing code, which avoids unnecessary merge commits. Use git merge to preserve the full, accurate chronological history of when branches were combined, making it safer for shared, public branches.`

      * Rebase: Best for updating your local feature branch with the latest main changes to keep history clean (linear). Never rebase shared/public branches.
     
      * Merge: Best for bringing finished feature branches back into the main branch, ensuring the history shows exactly when work was merged.
     
      * Use Rebase: When you want to update your feature branch with the main branch before submitting a pull request to keep a clean history.
     
      * Use Merge: When merging a completed feature branch into main or develop to preserve the history of the collaboration.

<hr/>

### Task 3: Squash Commit vs Merge Commit
`The main difference is that a merge commit preserves the entire history of all commits from the feature branch, while a squash commit combines all changes from a feature branch into a single, new commit`

* `Use a Merge Commit when` You want to maintain a complete and detailed history of all development steps, especially for projects where understanding the full context of changes and who made them is crucial for debugging and tracking.

* `Use a Squash Commit when` You want to keep your main branch history clean and easy to follow, and the individual work-in-progress commits on the feature branch are not valuable for the long-term project history. This is a common practice for short-lived feature or hotfix branches

1. Create a branch `feature-profile`, add 4-5 small commits (typo fix, formatting, etc.)

<img width="636" height="210" alt="image" src="https://github.com/user-attachments/assets/c3efdbff-bd9c-4938-9edf-25aa4be0a3c1" />

2. Merge it into `main` using `--squash` — what happens?

<img width="610" height="170" alt="image" src="https://github.com/user-attachments/assets/78aa646f-0d26-4f89-b84f-ce70b0bff6b8" />

3. Check `git log` — how many commits were added to `main`?

<img width="577" height="184" alt="image" src="https://github.com/user-attachments/assets/8d622373-1430-4d2d-b908-dc137760655d" />

4. Now create another branch `feature-settings`, add a few commits

<img width="585" height="249" alt="image" src="https://github.com/user-attachments/assets/bb48634e-1407-4845-a436-9d8be927031a" />

5. Merge it into `main` **without** `--squash` (regular merge) — compare the history

<img width="663" height="353" alt="image" src="https://github.com/user-attachments/assets/94216b0a-10fc-4c6d-a1fd-2970c018f091" />

6. Answer in your notes:
   - What does squash merging do?
      * Squash and merge is a Git workflow that takes all commits from a feature branch and compresses them into a single, new commit on the target branch (e.g., main) during a pull request.
      
      * It creates a cleaner, linear history by removing "work-in-progress" or intermediate commits, resulting in one tidy commit per feature.
    
      * When your feature branch has too many "fixed typo" or "work in progress" commits.
      
      * When you want a linear project history without complex branch merges. 
      
        
   - When would you use squash merge vs regular merge?

      * Use squash merge to combine all feature branch commits into one, creating a clean, linear main branch history, ideal for small features or messy, experimental work.
      
      * Use regular merge (merge commit) to preserve the entire detailed history, including intermediate steps and contributor context, which is better for complex, long-running, or multi-developer branches

<img width="641" height="242" alt="image" src="https://github.com/user-attachments/assets/00e8a7d6-038a-4386-bcd3-854cd3aea943" />

   - What is the trade-off of squashing?

      * Squashing in GitHub merges a feature branch into a single commit on the main branch, creating a clean, linear history, but at the cost of losing detailed, step-by-step commit history.
      
      * It simplifies project history and eases reversion, but makes debugging with git bisect harder and erases development context.

<hr/>

### Task 4: Git Stash — Hands-On
1. Start making changes to a file but **do not commit**
2. Now imagine you need to urgently switch to another branch — try switching. What happens?
3. Use `git stash` to save your work-in-progress
4. Switch to another branch, do some work, switch back

<img width="626" height="439" alt="image" src="https://github.com/user-attachments/assets/b90f1f44-586d-41f8-bcdd-263fa3212d33" />

5. Apply your stashed changes using `git stash pop`
6. Try stashing multiple times and list all stashes
7. Try applying a specific stash from the list

<img width="619" height="585" alt="image" src="https://github.com/user-attachments/assets/81c92bc2-d7a4-457f-ac08-41e3be127957" />

8. Answer in your notes:
   - What is the difference between `git stash pop` and `git stash apply`?
      * The main difference is that `git stash pop` applies the stashed changes to your working directory and then removes the stash entry from the stash list, while git `stash apply applies` the changes but leaves the stash in the list.
      * `git stash pop` applies stash changes to your working directory and deletes the stash entry.
      * `git stash apply` applies stash changes to your working directory but keeps the stash entry.
        
    - When would you use stash in a real-world workflow?
      * git stash is used to temporarily "shelve" or "stash" changes you've made to your working directory so you can work on something else, and then come back and re-apply them later. It is essential for maintaining a clean working directory without committing unfinished, messy, or broken code.
      *  Use `git stash` to save your work, switch to the main branch, fix the bug, commit it, and then switch back to your feature branch and use git stash pop to resume. 

   <img width="504" height="525" alt="image" src="https://github.com/user-attachments/assets/26425fc6-8da2-4c6d-8eb1-2ba70c042a04" />

<hr/>


### Task 5: Cherry Picking
1. Create a branch `feature-hotfix`, make 3 commits with different changes
2. Switch to `main`
3. Cherry-pick **only the second commit** from `feature-hotfix` onto `main`

<img width="513" height="90" alt="image" src="https://github.com/user-attachments/assets/92811052-afbf-4b33-8138-c567ed9683bd" />

4. Verify with `git log` that only that one commit was applied

<img width="602" height="395" alt="image" src="https://github.com/user-attachments/assets/0ea337de-8d17-47aa-80b5-ba45f6f67efa" />

5. Answer in your notes:
   - What does cherry-pick do?
      * In Git, `git cherry-pick` is a command that applies a specific commit from one branch onto your current HEAD branch.
      * It allows you to select individual, targeted changes (like a hotfix) rather than merging or rebasing an entire branch.
      * It is useful for moving mistakes to the correct branch or applying specific features, but can create duplicate commits if overused.
      * In Git, `git cherry-pick` is a powerful command that allows you to select specific commits from one branch and apply them individually to your current branch. 

   - When would you use cherry-pick in a real project?
      * In a real project, you would use git cherry-pick to apply specific, individual commits from one branch onto another without merging the entire branch.
      * It is commonly used for moving a critical bug fix from a development branch to a production release branch, fixing a commit made to the wrong branch, or pulling specific features into a shared branch

   - What can go wrong with cherry-picking?
      * Cherry-picking—the act of selecting only favorable data while ignoring contradictory evidence—leads to distorted, inaccurate, and misleading conclusions.
      * It undermines credibility, creates a false sense of reality, and can lead to bad decision-making in business, science, and daily life. It is often a form of confirmation bias.
      * If the same branch is merged then it will create duplicate commits.
      * It may create conflicts if the commit depends on previous commits.

