# 05 – Branching and Merging Practice

This guide walks through creating a new branch, making changes, committing, pushing, and merging into `main`.  
It includes **VS Code (GUI)** and **Terminal** methods side-by-side for easy reference.

---

## 🧩 What You’ll Learn
- Create and switch branches  
- Add and commit files  
- Push your branch to GitHub  
- Open and merge a Pull Request  
- Clean up branches after merging  

---

## ✅ Step 1: Make sure your repository is up to date
**VS Code GUI:**
1. Open your project folder in VS Code.  
2. Click the **Source Control** icon (left sidebar).  
3. Click **Sync Changes** (🔁) if you see it.  
4. Confirm you’re on the `main` branch (bottom-left corner).

**Terminal (inside VS Code or Git Bash):**
```bash
git checkout main
git pull origin main
```
## ✅ Step 2: Create a new branch

VS Code GUI:

Look at the bottom-left corner of VS Code — it shows your current branch (example: main).

Click that name → select Create new branch…

Name it something like:
```bash
feature/test-branch
```
✅ Step 3: Create a file and make a change

VS Code GUI:

In the Explorer panel (top-left), right-click your repo folder → select New File.

Name it:
```bash
test-branch-notes.md
```

Add this text:

### Test Branch Notes
This file was created to practice branching and merging in GitHub.


Save with Ctrl + S (Windows) or Cmd + S (Mac).

## ✅ Step 4: Stage and commit your changes

VS Code GUI: Click the Source Control icon (branch symbol on the left).

Hover over your new file and click the + to stage it.

Type a commit message, for example:

Add test-branch-notes.md for branch testing


Click the Commit (✔️) button or press Ctrl + Enter.

Terminal:
```bash
git add test-branch-notes.md
git commit -m "Add test-branch-notes.md for branch testing"
```

## ✅ Step 5: Push your branch to GitHub

VS Code GUI:

In the Source Control panel, click Publish Branch (or Sync Changes if you see it).

VS Code will upload your new branch to GitHub.

Terminal:
```bash
git push -u origin feature/test-branch
```
## ✅ Step 6: Create a Pull Request (PR)

On GitHub.com:

Go to your repository page.

You should see a yellow banner saying:

“feature/test-branch had recent pushes… Compare & pull request.”

Click Compare & pull request.

Make sure:

base branch: main

compare branch: feature/test-branch

Add a title like:

Add test-branch-notes.md for practice


Click Create Pull Request.

## ✅ Step 7: Merge the branch

Review the file changes shown.

Click Merge Pull Request → Confirm Merge.

You’ll see:

“Pull request successfully merged and closed.” 🎉

## ✅ Step 8: Clean up

On GitHub:
Click Delete branch (it appears right after merging).

Locally (VS Code or Terminal):
```bash
git checkout main
git pull origin main
git branch -d feature/test-branch
```
💡 Extra Tip: Why delete branches?

Deleting merged branches keeps your project tidy and avoids confusion later.
Your work is already safely stored in main after merging — no data is lost.

## ✅ Quick Summary Checklist

 Switch to main and pull latest changes

 Create new branch (GUI or terminal)

 Make and save edits

 Stage and commit changes

 Push branch to GitHub

 Open a Pull Request

 Merge into main

 Delete the branch (local + remote)