# Ollama on Mac Mini with Remote Access via SSH

This guide explains how to set up and run **Ollama** (a local LLM framework) on a **Mac Mini**, and access it from another laptop via SSH.

## 📌 Prerequisites
- A **Mac Mini** running macOS
- A **laptop** (Linux/macOS) to access it remotely
- SSH access to the Mac Mini

---

## 🚀 Step 1: Install Ollama on Mac Mini
1. Open **Terminal** on your Mac Mini.
2. Download and install Ollama:
   ```bash
   curl -fsSL https://ollama.com/install.sh | sh
   ```
3. Verify installation:
   ```bash
   ollama --version
   ```

4. Download LLM (mistral)
   ```bash
   ollama pull mistral
   ```

---

## 🔑 Step 2: Set Up SSH Key Authentication (Optional, Recommended)
To avoid entering passwords every time you connect via SSH, set up **key-based authentication**.

### **On Your Laptop** (Client)
1. Generate an SSH key (if not already created):
   ```bash
   ssh-keygen -t ed25519 -f ~/.ssh/ssh_key_mac_mini -C "Mac Mini Access"
   ```
2. Copy the public key to your Mac Mini:
   ```bash
   ssh-copy-id -i ~/.ssh/ssh_key_mac_mini.pub user@MAC_IP_ADDRESS
   ```
   *(Replace `user` with your Mac Mini username and `MAC_IP_ADDRESS` with its IP.)*

### **On Your Mac Mini**
Ensure SSH key authentication is enabled:
```bash
sudo nano /etc/ssh/sshd_config
```
Check for these lines:
```
PubkeyAuthentication yes
PasswordAuthentication no
```
Restart SSH:
```bash
sudo launchctl stop com.openssh.sshd
sudo launchctl start com.openssh.sshd
```

---

## 🌐 Step 3: Start Ollama Server on Mac Mini
Run Ollama in the background:
```bash
ollama serve &
```
To check if it's running:
```bash
ps aux | grep ollama
```

---

## 🔗 Step 4: Access Ollama Remotely via SSH
### **On Your Laptop**
1. SSH into your Mac Mini:
   ```bash
   ssh -i ~/.ssh/ssh_key_mac_mini user@MAC_IP_ADDRESS
   ```
2. Send a request to Ollama (example with `mistral` model):
   ```bash
   curl http://localhost:11434/api/generate -d '{"model": "mistral", "prompt": "Hello!"}'
   ```

---

## 🛠️ Troubleshooting
### **Find Mac Mini's IP Address**
```bash
ipconfig getifaddr en0
```

### **Check SSH Logs (Mac Mini)**
```bash
sudo log stream --process sshd
```

### **Manually Add SSH Key to Agent (Laptop)**
```bash
ssh-add ~/.ssh/ssh_key_mac_mini
```

---

## 🎯 Summary
- Installed **Ollama** on Mac Mini
- Set up **SSH key authentication**
- Started Ollama server and accessed it via SSH from another laptop

Now you can use your **Mac Mini** as a remote **local LLM server**! 🚀

