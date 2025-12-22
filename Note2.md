# Git Workshop
### Creating a New Repository (Remote)
To create a new Git repository, follow these steps:
1. Go to GitHub/Gitlab.
2. สร้าง repository ใหม่
3. ตั้งชื่อ repository และตั้งค่าเป็น public/private ตามต้องการ (เลือก private)
4. เอา Initialize this repository with a README ออก (สร้างเองทีหลัง)
5. คลิก Create repository

### Configure tooling (First time)
ใช้คำสั่งใน terminal:
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Create Repositories
ใช้คำสั่งใน terminal (Local with project name):
```bash
git init [project-name]
```

ใช้คำสั่งใน terminal (Clone from remote):
```bash
git clone [url]
```

### Make Changes
ใช้คำสั่งใน terminal (Lists all new or modified files to be committed):
```bash
git status
```

ใช้คำสั่งใน terminal (Shows file differences not yet staged):
```bash
git diff
```

ใช้คำสั่งใน terminal (Snapshots the file in preparation for versioning):
```bash
git add [file]
git add readme.txt
git add .
```

### Make Change #2
ใช้คำสั่งใน terminal (Show file differences between staging and the last file version):
```bash
git diff --staged
```

Unstages the file but preserves its contents (เป็นการ Reset การ add):
```bash
git reset [file]
git reset readme.txt
```

Records file snapshots permanently in version history (การใช้ commit + commit message):
```bash
git commit -m "Your commit message"
```

### Group Changes #3
lists local branches ทั้งหมด:
```bash
git branch
```

New branch:
```bash
git branch [branch-name]
```

Change branch:
```bash
git checkout [branch-name]
```

### Group Changes #4
Merge branch:
```bash
git merge [branch-name]
``` 

Delete branch:
```bash
git branch -d [branch-name]
```

### Refactor Filenames
Delete file (Stage the file for removal):
```bash
git rm [file]
```

Delete file but keep it locally:
```bash
git rm --cached [file]
```

Rename file:
```bash
git mv [old-filename] [new-filename]
```

### Suppress Tracking
ใช้ .gitignore File เพื่อบอก git ว่าไม่ต้องติดตามไฟล์หรือโฟลเดอร์ไหนบ้าง
```bash
# ใน .gitignore (ไม่ต้องการ Folder .venv, __pycache__)
.venv/
__pycache__/
```

List all ignored files:
```bash
git ls-files --others -i --exclude-standard
```

### Save Fragments
Temp stash (Save ที่ยังไม่ commit):
```bash
git stash
```

Restore stash:
```bash
git stash pop
```

List stashes:
```bash
git stash list
```

### Review History
Show commit history:
```bash
git log
```

Lists version history for a file (including renames)
```bash
git log --follow [file]
```

Show diff between branches:
```bash
git diff [branch-1]..[branch-2]
```

### Redo Commits
Undo all commits after [commit], แต่เก็บ changes locally:
```bash
git reset [commit]
```

Discard all history and changes to commit ที่ต้องการ (Hard reset):
```bash
git reset --hard [commit]
```

### Synchronize Changes
Download all history from repo bookmark:
```bash
git fetch [bookmark-name]
```

Combine Branch (MERGE) from remote to local:
```bash
git merge [bookmark-name]/[branch-name]
```

Upload local commits to remote repo:
```bash
git push [alias] [branch]
git push origin main
```

Download bookmark history และ merge เข้ากับ local branch:
```bash
git pull
```

# Automate Testing with PYTHON (pytest)
### Create Python Project with uv
เริ่มจากการ Install จากนั้น add สิ่งที่จำเป็น
แล้วลองใช้งาน โดยการเขียน code ง่ายๆ แล้วทดสอบ

### Run app
ใช้คำสั่งใน terminal:
```bash
uv run streamlit run main.py
```

# How to automate testing with PYTEST
### Basic Rules
1. ตั้งชื่อไฟล์ทดสอบให้ขึ้นต้นด้วย test_ หรือ ลงท้ายด้วย _test.py
2. ตั้งชื่อฟังก์ชันทดสอบให้ขึ้นต้นด้วย test_
3. หากเขียนใน class ต้องตั้งชื่อ class ให้ขึ้นต้นด้วย Test

### การเขียน Unit Test
ตัวอย่างการเขียน Unit Test ง่ายๆ
```python
# File test_basic.py
def add(a, b):
    return a + b

def test_add_positive_numbers():
    result = add(2, 3)
    assert result == 5 # Check if result is 5
```

### การรัน Unit Test
ใช้คำสั่งใน terminal (จะเป็นการ run test ทั้งหมดใน Folder ปัจจุบัน):
```bash
pytest
```
ถ้าอยากให้แสดงแบบละเอียดให้เพิ่ม -v ตามหลัง pytest

### Run Test (Specific File)
ใช้ pytest ตามด้วยชื่อ File นั้นๆ

### Project Structure 
ตัวอย่างโครงสร้าง Project ที่ดี
```
src (folder) - เก็บโค้ดหลักของโปรเจค
tests (folder) - เก็บโค้ดสำหรับทดสอบ
pytest.ini (file)
requirements.txt (file)
```

# Pytest Workshop
TDD - Test Driven Development
1. Create file test_transformations.py (เกี่ยวกับพวก normalize_columns, replace_percent)
2. Create file transformations.py (Functions ที่จะทดสอบ)
3. Run pytest