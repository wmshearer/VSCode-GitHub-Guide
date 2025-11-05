# Step 6
Fork the repository on GitHub, then clone your fork locally.

git clone <REPOSITORY_URL>
cd <PROJECT_FOLDER>

Create a new branch to work safely.
git checkout -b <BRANCH_NAME>

Make your edits, then stage and commit.
git add .
git commit -m "Fix typo in README"

Push your branch to your fork.
git push --set-upstream origin <BRANCH_NAME>

Open a Pull Request on GitHub.
Use a clear title, short description, and link issues like: Closes #42


# Step 7
After opening a PR, GitHub may run automated checks.
If any fail, fix the problem locally, then push again.

git add .
git commit -m "Fix failing test"
git push

Ask for help in PR comments if unsure what failed.


# Step 8
Keep commit messages short, clear, and in present tense.
Do not end with a period.

git commit -m "Add PowerShell example for setup"

Example PR body:
What changed: Added PowerShell setup example
Why: Clarifies installation steps for Windows users
Related issue: Closes #58


# Step 9
Add reviewers if needed.
Add labels like bug, enhancement, or documentation.
Link issues so merging will close them automatically.
Example: Closes #17


# Step 10
Make requested changes, then push again.

git add .
git commit -m "Apply review feedback"
git push

The PR updates automatically.
Once approved, it will be merged.


# Step 11
Check Insights → Contributors.
Follow maintainers or organizations.
Join community chats or attend events.
Keep learning and engaging with others.


# Step 12
You can:
1. Publish a standalone library
2. Maintain a fork with your improvements
3. Create a GitHub Action and publish it

Once you publish something, you’re a maintainer.
Set clear expectations in your README.

Quick checklist:
LICENSE present  
Active issues and PRs  
Beginner-friendly labels  
Code of Conduct  
CONTRIBUTING guidelines  

Final thoughts:
Contributing to open source is about collaboration and growth.
Start small, communicate clearly, and keep learning. 🌱
