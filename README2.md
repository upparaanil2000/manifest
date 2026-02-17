# Repository Setup and Cloning Guide (WSL/VS Code) [

> **Detailed step-by-step guide to set up the Repo tool and clone SDV automotive workspaces (`cluster_ws` and `infotainment_ws`) using WSL via VS Code (recommended) or Git Bash on Windows.** [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87735813/13bd4330-9078-40e1-b7b3-cb0bb7dd2edd/Repository-Setup-and-Cloning-Guide_Readmefile.odt)

## 🎯 For QA Automation Engineers (Automotive/Embedded)

**Target Audience:** Manual testers transitioning to test automation, Yocto builds, Android Automotive (AAOS), Renesas R-Car V4H development.

***

## 📋 Table of Contents
- [Prerequisites](#prerequisites)
- [SSH Key Authentication (REQUIRED)](#ssh-key-authentication-required)
- [Cloning Workspaces](#cloning-workspaces)
- [Accessing Files](#accessing-files)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

***

## Prerequisites

### Option A: WSL via VS Code ⭐ **RECOMMENDED**

**WSL provides full Linux environment for Yocto builds & automotive source management.**

#### 1. Install WSL2
```powershell
# PowerShell as Administrator
wsl --install
```
- Restart PC
- Launch Ubuntu (Start Menu) → Set username/password
- Verify: `wsl -l -v` (WSL 2)

#### 2. Install VS Code + WSL Extension
1. [Download VS Code](https://code.visualstudio.com)
2. Extensions (Ctrl+Shift+X) → Search "WSL" → Install
3. Ctrl+Shift+P → "WSL: Connect to WSL" → New window (WSL: Ubuntu)

#### 3. Install Dependencies
```bash
sudo apt update
sudo apt install git python3 curl -y
```

#### 4. Setup Repo Tool
```bash
mkdir -p ~/bin
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
echo 'export PATH=~/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
repo --version  # ✅ Verify
```

### Option B: Git Bash (Windows Native)
⚠️ **Run as Administrator for cloning**

1. [Download Repo](https://storage.googleapis.com/git-repo-downloads/repo)
2. Create `C:\Users\YourName\bin` → Paste `repo`
3. **Add to PATH:** Environment Variables → System PATH → Add bin folder
4. [Install Python](https://python.org) (check "Add to PATH")

***

## SSH Key Authentication (REQUIRED)

**For private GitLab repos: `git@gitlab.rampgroup.com:sdv/v2/manifest.git`**

### 1. Generate Private + Public Keys
```bash
cd ~
ssh-keygen -t rsa -b 4096 -C "your_gitlab_email@rampgroup.com"
```
```
Enter file: [ENTER] (default)
Enter passphrase: [strong password OR ENTER]
```

**✅ Creates:**
```
~/.ssh/
├── id_rsa      # PRIVATE - NEVER SHARE
└── id_rsa.pub  # PUBLIC - UPLOAD TO GITLAB
```

### 2. Secure Permissions
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
```

### 3. Copy Public Key
```bash
# WSL: Manual copy
cat ~/.ssh/id_rsa.pub

# Git Bash: Clipboard
cat ~/.ssh/id_rsa.pub | clip
```

### 4. Add to GitLab
```
https://gitlab.rampgroup.com
Avatar → Preferences → SSH Keys
PASTE: id_rsa.pub content
Title: "WSL-Repo-Key"
Add SSH Key ✅
```

### 5. Load Private Key
```bash
# WSL
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa

# Git Bash
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_rsa
```

### 6. Verify
```bash
ssh -T git@gitlab.rampgroup.com
# ✅ "Welcome to GitLab, @username!"
```

***

## Cloning Workspaces

### Environment 1: WSL (VS Code Terminal) ⭐ RECOMMENDED
**Open VS Code → Ctrl+Shift+` (WSL Terminal)**

#### Cluster Workspace (Instrument Cluster)
```bash
cd ~
mkdir cluster_ws && cd cluster_ws
repo init -u git@gitlab.rampgroup.com:sdv/v2/manifest.git -m cluster.xml
repo sync -j$(nproc)  # 15-45 min, 10-20GB
```

#### Infotainment Workspace (Car Entertainment)
```bash
cd ~
mkdir infotainment_ws && cd infotainment_ws
repo init -u git@gitlab.rampgroup.com:sdv/v2/manifest.git -m infotainment.xml
repo sync -j$(nproc)  # 30-90 min, 20-50GB
```

### Environment 2: Git Bash (Administrator)
```
Right-click Git Bash → "Run as Administrator"
```
```bash
cd ~
mkdir cluster_ws && cd cluster_ws
repo init -u git@gitlab.rampgroup.com:sdv/v2/manifest.git -m cluster.xml
repo sync -j8  # Windows limit

# Repeat for infotainment_ws
```

***

## Accessing Files

| Environment | Method |
|-------------|--------|
| **WSL** | `explorer.exe .` (Windows Explorer) |
| **Windows** | `\\wsl$\Ubuntu\home\username\` |
| **Git Bash** | Direct Windows paths |

***

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `repo: command not found` | `source ~/.bashrc` or check PATH |
| `Permission denied (publickey)` | `ssh -T git@gitlab.rampgroup.com` |
| `repo sync` slow | `repo sync -j$(nproc)` (parallel) |
| **Git Bash permissions** | **Run as Administrator** |
| VS Code terminal fails | Use standalone Git Bash (Admin) |

***

## Next Steps (Post-Cloning)

```bash
cd ~/cluster_ws
source oe-init-build-env build  # Yocto setup
# Edit conf/local.conf for Renesas R-Car V4H
bitbake core-image-weston
```

***

## 📊 Expected Results

| Workspace | Size | Time |
|-----------|------|------|
| `cluster_ws` | 10-20GB | 15-45 min |
| `infotainment_ws` | 20-50GB | 30-90 min |
| **TOTAL** | **30-70GB** | **1-2 hours** |

**✅ Ready for Yocto builds, AAOS testing, automotive infotainment development!**

***

*Optimized for QA engineers in Hyderabad automotive companies transitioning to embedded test automation.* [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87735813/13bd4330-9078-40e1-b7b3-cb0bb7dd2edd/Repository-Setup-and-Cloning-Guide_Readmefile.odt)
