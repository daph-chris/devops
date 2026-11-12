# 1. Configure Git
git config --global user.name "Daphne Christina Nelson"
git config --global user.email "YOUR_GITHUB_EMAIL"

# 2. Create folder
mkdir Devops-Ex1
cd Devops-Ex1

# 3. Initialize repository
git init
git branch -M main

# 4. Create Python files
notepad ex.py
notepad ex1.py

# 5. Add and commit
git status
git add .
git commit -m "Initial commit"

# 6. Connect GitHub
git remote add origin https://github.com/Daph-Chris/Devops-Ex1.git
git remote -v

# 7. Push main branch
git push -u origin main

# 8. Create new branch
git checkout -b main1

# 9. Modify ex.py using Notepad
notepad ex.py

# 10. Commit changes
git status
git add ex.py
git commit -m "commit"

# 11. Push new branch
git push origin main1

# 12. Create and merge Pull Request on GitHub

# 13. Pull merged changes
git checkout main
git pull origin main

# 14. Verify
git status
git log --oneline
python ex.py
