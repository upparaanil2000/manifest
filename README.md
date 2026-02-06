```markdown
# Repository Setup and Cloning Guide

This guide describes the prerequisites and step-by-step process to set up the **Repo tool** and clone required workspaces using either **WSL (recommended)** or **Git Bash on Windows**.

---

## 1. Prerequisites

You can use either **WSL via VS Code** or **Git Bash on Windows**.  
WSL is recommended for a smoother Linux-like development experience.

### Option A: Using WSL via VS Code (Recommended)

#### Install Required Tools
1. Install **Visual Studio Code**  
2. Install the **WSL extension** in VS Code  
3. Press `Ctrl + Shift + P` → select **Connect to WSL**  
4. Update packages and install dependencies:

```bash
sudo apt update
sudo apt install git python3 curl -y
```

#### Repo Tool Setup

```bash
mkdir -p ~/bin
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
echo 'export PATH=~/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

---

### Option B: Using Git Bash (Windows)

#### Download Repo Tool
Download the Repo tool from:  
[https://storage.googleapis.com/git-repo-downloads/repo](https://storage.googleapis.com/git-repo-downloads/repo)

#### Add Repo Tool to PATH
- Place the downloaded `repo` file inside a **bin** directory  
- Add this **bin** directory to your Windows **PATH** (mandatory)

#### Install Python
- Install **Python 3.11 or later**  
- Ensure Python is added to the system **PATH**

---

## 2. Cloning Process

⚠️ **Important:**  
All cloning environments (WSL or Git Bash) must be authenticated using **SSH keys**.

### 2.1 Cluster Workspace

```bash
mkdir ~/cluster_ws
cd ~/cluster_ws
repo init -u git@gitlab.rampgroup.com:sdv/v2/manifest.git -m cluster.xml
repo sync
```

### 2.2 Infotainment Workspace

```bash
mkdir ~/infotainment_ws
cd ~/infotainment_ws
repo init -u git@gitlab.rampgroup.com:sdv/v2/manifest.git -m infotainment.xml
repo sync
```

---

## 3. Notes
- To locate cloned files on your Windows system:  
  - **VS Code (WSL):** run `explorer.exe`
- Ensure **SSH access** is configured before running `repo init`
- Repo tool uses the **manifest file (.xml)** to clone multiple repositories at once
```
