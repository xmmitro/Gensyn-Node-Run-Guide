# 🚀 Gensyn Node Setup Guide by 0xSumitro

Welcome to the ultimate guide for running a **Gensyn node** on a VPS or cloud GPU! This step-by-step tutorial will help you set up, run, and troubleshoot your node to contribute to the decentralized AI compute network. Whether you're a beginner or a pro, this guide has you covered. Let's dive in! 🌟

---

## 📋 Table of Contents

1. **[System Requirements](#%EF%B8%8F-system-requirements)**  
2. **[Installation](#-installation)**   
3. **[Running the Node](#-running-the-node)**   
4. **[Backing Up Your `swarm.pem` File](#-backing-up-your-swarmpem-file)**  
5. **[Troubleshooting Common Errors](#%EF%B8%8F-troubleshooting-common-errors)**  
   5.1 [PS1: unbound variable error](#1%EF%B8%8F%E2%83%A3-ps1-unbound-variable-error)  
   5.2 [P2P Daemon Error](#2%EF%B8%8F%E2%83%A3-p2p-daemon-error)  
   5.3 [Line 101 / 121: open: command not found](#3%EF%B8%8F%E2%83%A3-line-101--121-open-command-not-found)
---

## 🖥️ System Requirements
Before you start, ensure your system meets these requirements:

| **Requirement**   | **Details**                                                                       |
|-------------------|-----------------------------------------------------------------------------------|
| **OS**            | Ubuntu 20.04 or later (Linux-based VPS or cloud GPU instance)                     |
| **RAM**           | At least 16 GB (minimum 24 GB VRAM for GPU instances)                              |
| **CPU**           | 8 cores or more                                                                   |
| **Storage**       | minimum 50 GB free space                                                          |
| **GPU**           | NVIDIA GPU with CUDA support (e.g., RTX 4090, A100, H100) for optimal performance |
| **Network**       | Stable internet connection with at least 100 Mbps                                 |

> 💡 **Tip**: For the best performance, rent a GPU instance from providers like [RunPod](https://www.runpod.io/) or [Hyperbolic Labs](https://www.hyperbolic.xyz/). A VPS can work, but you may face out-of-memory (OOM) errors or lower winning rates.

---

## 📥 Installation
Follow these steps to set up your environment. All commands assume you're using a Linux-based system (e.g., Ubuntu).

### 🌟 Step 1: Update System and Install `sudo` (Cloud GPUs Only)
```bash
sudo apt update && sudo apt install -y sudo
```

### 🌟 Step 2: Install Core Dependencies
Install essential packages for running the node:
```bash
sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof nano unzip iproute2
```

### 🌟 Step 3: Install Node.js, npm, and Yarn
Node.js and Yarn are required for the `rl-swarm` repository.
**For Linux/WSL/Cloud GPU:**
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - && \
sudo apt update && sudo apt install -y nodejs
```
**Install Yarn:**
```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list > /dev/null
sudo apt update && sudo apt install -y yarn
```

### 🌟 Step 4: Verify Versions
Check that Node.js, npm, and Yarn are installed correctly:
```bash
node -v && npm -v && yarn -v
```

> *"✅ *Expected Output: You should see version numbers (e.g., v20.x.x for Node.js, 10.x.x for npm, 1.x.x for Yarn)."**

---

## 🚀 Running the Node
Now that your environment is set up, let's run the Gensyn node!

### 1️⃣ Clone the `rl-swarm` Repository
```bash
git clone https://github.com/gensyn-ai/rl-swarm.git
```

### 2️⃣ Create a Screen Session (VPS & Cloud GPU Only)
Run the node in a detachable ``screen`` session to keep it running in the background:
```bash
screen -S gensyn
```

> 💡 *Tip: To reattach to the session later, use screen -r gensyn. To exit without stopping, press Ctrl+A, then D.*

### 3️⃣ Navigate to the `rl-swarm` Directory
```bash
cd rl-swarm
```

### 4️⃣ Handle the `swarm.pem` File (Important for VPS/GPU)
- **First-time setup**: Proceed to the next step.
- **Switching machines**: If you have a `swarm.pem` file from another machine, copy it to the `rl-swarm` directory.

### How to Copy `swarm.pem`:
1. On your local machine, use SCP or drag-and-drop the file to your VPS/GPU instance.
2. Example SCP command:
```bash
scp swarm.pem username@your-vps-ip:/path/to/rl-swarm/
```
3. Verify the file is in the `rl-swarm` directory:
```bash
ls -l swarm.pem
```
> *⚠️ Warning: The swarm.pem file is critical for your node’s identity. Back it up securely! See the  below.*


### 5️⃣ Set Up the Virtual Environment and Run the Node
```bash
python3 -m venv .venv
source .venv/bin/activate
./run_rl_swarm.sh
```

### 6️⃣ Log In to Your Gensyn Node
- Open the URL provided in the terminal (e.g., http://localhost:3000).
- Choose "Sign in with email" (Google Auth may fail due to localhost restrictions).
- Check your email for a login link and authenticate.
- Your node should now be running and connected to the Gensyn TestNet! 🎉
  
> *💡 Tip: Monitor your node’s performance and leaderboard ranking via the Gensyn dashboard.*

## 💾 Backing Up Your swarm.pem File
The `swarm.pem` file is your node’s identity. Losing it means losing your contributions! Follow these steps to back it up:
1. Locate the File:
```bash
ls -l ~/rl-swarm/swarm.pem
```
2. **Copy to Your Local Machine**: Use SCP to transfer the file:
```bash
scp username@your-vps-ip:~/rl-swarm/swarm.pem ./swarm.pem
```
> *📥 Save the files to a secure location.*

## 🛠️ Troubleshooting Common Errors
Here are solutions to common errors you might encounter, additional scenarios based on community reports and GPU/VPS issues.

### 1️⃣ PS1: unbound variable error
![Screenshot 2025-04-26 132240](https://github.com/user-attachments/assets/7fa4135a-af5f-4aec-b70b-95c70352b0a5)

Solution:
Modify the .bashrc file to handle the unbound variable:
```bash
sed -i '/$$  -z "\$PS1"  $$ && return/i : "${PS1:=}"' /root/.bashrc
```
Restart the node:
```bash
./run_rl_swarm.sh
```

### 2️⃣ P2P Daemon Error
![image](https://github.com/user-attachments/assets/fd6fc8a0-e4c9-4b9d-8c14-5b5a6db0f387)


Solution:
Increase the P2P daemon startup timeout:
```bash
sed -i -E 's/(startup_timeout: *float *= *)[0-9.]+/\1120/' $(python3 -c "import hivemind.p2p.p2p_daemon as m; print(m.__file__)")
```
Press `Ctrl+C` to stop the node, then restart:
```bash
./run_rl_swarm.sh
```

### 3️⃣ Line 101 / 121: open: command not found

Solution:
Comment out the problematic lines in `run_rl_swarm.sh`:
- For line 101:
```bash
sed -i '101s|^|# |' run_rl_swarm.sh
```
- For line 121:
```bash
sed -i '121s|^|# |' run_rl_swarm.sh
```
Restart the node:
```bash
./run_rl_swarm.sh
```

# 🌟 Good Luck in the Swarm! ❤️



































