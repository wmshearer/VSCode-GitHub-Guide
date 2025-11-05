# Branch Workflow (Always Branch, Never Main)

## Why
- `main` stays stable
- You work safely on a feature branch
- Merge via Pull Request (PR)

## Start a feature (GUI + one terminal session)
1) **Create branch (GUI)**  
   - Bottom-left branch name → **Create new branch…**  
   - Name it: `feature/<short-topic>` (e.g., `feature/github-flow-guide`)

2) **Work → stage → commit (GUI)**  
   - Edit files → **Ctrl + S**  
   - **🌿 Source Control** → **+** stage → message → **✔ Commit**

3) **Push + open PR (GUI)**
   - **… → Push** (or **Publish Branch**) → click **Open on GitHub** → **Compare & pull request**

4) **Merge on GitHub**
   - Review → **Merge pull request** → **Confirm**

5) **Sync and clean locally** (ONE terminal window)
```bash
# make sure you're in the repo folder (VS Code → File → Open Folder…)

# 1) Ensure main is up to date before starting work
git checkout main
git pull

# 2) Create your feature branch (if not created via GUI)
git checkout -b feature/github-flow-guide

# … do work in VS Code, then stage/commit in the GUI …

# 3) Push your feature branch (first time sets upstream)
git push -u origin feature/github-flow-guide

# 4) After merging the PR on GitHub, sync + clean
git checkout main
git pull
git branch -d feature/github-flow-guide
