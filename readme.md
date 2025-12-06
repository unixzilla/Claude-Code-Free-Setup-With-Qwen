# 🚀 Claude Code + Qwen — Complete Free Setup
![Free Claude Code](https://img.shields.io/badge/Claude_Code-Free_Qwen-orange?style=for-the-badge&logo=alibabacloud)
**Universal Guide: Windows (PowerShell + CMD), macOS, Linux**

This guide provides the simplest and most reliable method to run **Claude Code completely FREE** using the Qwen API via `claude-code-router`.

✅ Works perfectly on:
✔ Windows PowerShell
✔ Windows CMD
✔ macOS Terminal
✔ Linux Bash

All steps tested & verified as of December 2025.

⚠️ **Most Common Problem Fixed**
Other guides may use OS-specific commands or assume prior setup, which can cause confusion. This guide gives **100% correct commands for each OS separately**.

## 📌 Table of Contents
- [Step 0 — Prerequisites](#step-0--prerequisites)
- [Step 1 — Authenticate Qwen & Get Token](#step-1--authenticate-qwen--get-token)
- [Step 2 — Install Claude Tools](#step-2--install-claude-tools)
- [Step 3 — Create Required Folders](#step-3--create-required-folders)
- [Step 4 — Create config.json](#step-4--create-configjson-correct-for-each-os)
- [Step 5 — Add Your Access Token](#step-5--add-your-access-token)
- [Step 6 — Verify Installation](#step-6--verify-installation)
- [Step 7 — Daily Usage](#step-7--daily-usage)
- [Troubleshooting: Token Refresh](#troubleshooting-token-refresh)
- [Author](#author)

---

## 🔥 STEP 0 — PREREQUISITES

### 1. Check Node.js
Open your terminal and run:
```bash
node --version
```
This must show `v18.0.0` or higher. If not, install it from [nodejs.org](https://nodejs.org).

### 2. Install Qwen CLI
```powershell
npm install -g @qwen-code/cli
```

---

## 🔥 STEP 1 — AUTHENTICATE QWEN & GET TOKEN

### 1. Log In
In your terminal, run the following command. This will open a browser window for you to log in and authorize the CLI.
```bash
qwen login
```

### 2. Find Your Credentials File
After logging in, the CLI creates a file containing your access token.
- **Windows:** `%USERPROFILE%\.qwen\oauth_creds.json`
- **macOS/Linux:** `~/.qwen/oauth_creds.json`

Open this file. It will look like this:
```json
{
  "access_token": "ey...",
  "token_type": "Bearer",
  "refresh_token": "ey...",
  "resource_url": "portal.qwen.ai",
  "expiry_date": 1764876220290
}
```

### 3. Copy Your Access Token
Copy the entire `access_token` value. You will need it in Step 5.

---

## 🔥 STEP 2 — INSTALL CLAUDE TOOLS

### Windows (PowerShell or CMD):
```powershell
npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router
```
### macOS / Linux:
```bash
sudo npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router
```

---

## 🔥 STEP 3 — CREATE REQUIRED FOLDERS

### Windows PowerShell:
```powershell
New-Item -ItemType Directory -Force -Path $HOME\.claude-code-router
New-Item -ItemType Directory -Force -Path $HOME\.claude
```
### Windows CMD:
```cmd
mkdir %USERPROFILE%\.claude-code-router
mkdir %USERPROFILE%\.claude
```
### macOS / Linux:
```bash
mkdir -p ~/.claude-code-router ~/.claude
```
*The `-Force` / `-p` flags prevent errors if the folders already exist.*

---

## 🔥 STEP 4 — CREATE `config.json` (Correct For Each OS)

### 🟦 Windows (PowerShell + CMD) — Use Notepad
Run this command to open Notepad with the correct file path:
```powershell
notepad $HOME\.claude-code-router\config.json
```
Or in CMD:
```cmd
notepad %USERPROFILE%\.claude-code-router\config.json
```
Paste this exact JSON content into Notepad, then **save and close** the file.
```json
{
  "LOG": true,
  "LOG_LEVEL": "info",
  "HOST": "127.0.0.1",
  "PORT": 3456,
  "API_TIMEOUT_MS": 600000,
  "Providers": [
    {
      "name": "qwen",
      "api_base_url": "https://portal.qwen.ai/v1/chat/completions",
      "api_key": "YOUR_QWEN_ACCESS_TOKEN_HERE",
      "models": [
        "qwen-coder"
      ]
    }
  ],
  "Router": {
    "default": "qwen,qwen-coder",
    "background": "qwen,qwen-coder",
    "think": "qwen,qwen-coder",
    "longContext": "qwen,qwen-coder",
    "longContextThreshold": 60000,
    "webSearch": "qwen,qwen-coder"
  }
}
```

### 🟩 macOS / Linux — Use `cat`
Copy and paste this entire block into your terminal and press Enter:
```bash
cat > ~/.claude-code-router/config.json << 'EOF'
{
  "LOG": true,
  "LOG_LEVEL": "info",
  "HOST": "127.0.0.1",
  "PORT": 3456,
  "API_TIMEOUT_MS": 600000,
  "Providers": [
    {
      "name": "qwen",
      "api_base_url": "https://portal.qwen.ai/v1/chat/completions",
      "api_key": "YOUR_QWEN_ACCESS_TOKEN_HERE",
      "models": [
        "qwen-coder"
      ]
    }
  ],
  "Router": {
    "default": "qwen,qwen-coder",
    "background": "qwen,qwen-coder",
    "think": "qwen,qwen-coder",
    "longContext": "qwen,qwen-coder",
    "longContextThreshold": 60000,
    "webSearch": "qwen,qwen-coder"
  }
}
EOF
```

---

## 🔥 STEP 5 — ADD YOUR ACCESS TOKEN
Now, replace the placeholder in `config.json` with the token you copied in Step 1.

### Easiest way to edit:
- **Windows (PowerShell/CMD):**
  ```powershell
  notepad $HOME\.claude-code-router\config.json
  ```
- **macOS/Linux:**
  ```bash
  nano ~/.claude-code-router/config.json
  ```
In the file, find the line `"api_key": "YOUR_QWEN_ACCESS_TOKEN_HERE",` and replace `YOUR_QWEN_ACCESS_TOKEN_HERE` with your actual token. **Save and exit.**

---

## 🔥 STEP 6 — VERIFY INSTALLATION
Run these commands. They should output version numbers, not errors.
```bash
claude --version
ccr version
```
If you get a "command not found" error, close and reopen your terminal.

---

## 🔥 STEP 7 — DAILY USAGE
You need two terminals running.

### Terminal 1 — Start the Router
```bash
ccr start
```
Wait for it to say the service has started.

### Terminal 2 — Start Claude Code
Open a **new** terminal window or tab.

#### Windows PowerShell:
```powershell
$env:ANTHROPIC_AUTH_TOKEN = "test"
$env:ANTHROPIC_API_KEY = ""
$env:ANTHROPIC_BASE_URL = "http://127.0.0.1:3456"
claude
```

#### Windows CMD:
```cmd
set ANTHROPIC_AUTH_TOKEN=test
set ANTHROPIC_API_KEY=
set ANTHROPIC_BASE_URL=http://127.0.0.1:3456
claude
```

#### macOS / Linux (Recommended):
```bash
eval "$(ccr activate)"
claude
```
**Test it:** Type `hi` and press Enter. Qwen should reply via Claude Code. 🎉

---

## ⚠️ Troubleshooting: Token Refresh
The Qwen access token expires after some time. If you see `401 Unauthorized` errors, you need to refresh it.

1.  **Re-authenticate:** Run `qwen login` again in your terminal. This will update your `oauth_creds.json` file with a new token.
2.  **Copy New Token:** Open `oauth_creds.json` (as in Step 1) and copy the new `access_token`.
3.  **Update Config:** Paste the new token into your `config.json` file, replacing the old one (as in Step 5).
4.  **Restart Router:**
    ```bash
    ccr restart
    ```
Your connection should now work again.

## 🙌 Author
Hamid Raza
Feel free to ⭐ star this repo if it helped you!

Made with ❤️ for the community — Enjoy unlimited free Claude Code!