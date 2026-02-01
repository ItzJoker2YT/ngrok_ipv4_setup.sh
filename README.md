
# 🚀 Ngrok SSH Tunnel Quick-Setup

A Bash script to instantly turn any Linux machine (VPS, VM, or Google Colab) into an SSH-accessible server using an Ngrok tunnel.

![Bash](https://img.shields.io/badge/Language-Bash-4EAA25?style=flat-square)
![Ngrok](https://img.shields.io/badge/Service-Ngrok-1F1E37?style=flat-square)
![Root](https://img.shields.io/badge/Requires-Sudo-red?style=flat-square)

## ⚡ Quick Start

Run this single command on your Linux machine. It downloads the script to a temporary file and runs it.

```bash
curl -sL https://bit.ly/3Of0vb8 -o setup.sh && sudo bash setup.sh

```

### What happens next?

1. **Dependencies:** The script installs `openssh-server`, `curl`, `wget`, and `tar`.
2. **Password:** It asks you to set a **custom root password**.
3. **Auth:** It asks for your **Ngrok Authtoken** (Get it [here](https://dashboard.ngrok.com/get-started/your-authtoken)).
4. **Connect:** It outputs a connection command like:
> `ssh root@0.tcp.ngrok.io -p 12345`



---

## 🛠️ How It Works

This script automates the tedious process of exposing a local server to the internet. Here is the logic flow:

1. **Backup:** Backs up your current SSH config to `/etc/ssh/sshd_config.bak`.
2. **Unlock SSH:**
* Enables `PermitRootLogin yes` (allows root login).
* Enables `PasswordAuthentication yes` (allows password login without keys).
* Restarts the SSH service.


3. **Install Ngrok:** Detects your CPU architecture (AMD64 or ARM64) and downloads the correct binary.
4. **Tunnel:** Authenticates with your token and starts a TCP tunnel on port 22 in the background.
5. **API Fetch:** Queries the local Ngrok API (`localhost:4040`) to find the public URL/Port and displays it.

---

## ⚠️ Security & Safety Disclosure

**Is this safe?**
Yes, for temporary environments (like Google Colab, a fresh VPS, or a testing VM). **Do not use this on a production server without understanding the risks.**

| Risk | Description | Mitigation |
| --- | --- | --- |
| **Root Login** | This script enables Root login via password. | The script forces you to set a password immediately. **Make it strong.** |
| **Config Changes** | Your `/etc/ssh/sshd_config` is modified. | A backup is created at `/etc/ssh/sshd_config.bak`. |
| **Ngrok Token** | Your token is saved in plain text in `~/.config`. | This is standard Ngrok behavior. Do not share the machine with untrusted users. |

---

## 📋 Prerequisites

* A Linux environment (Ubuntu/Debian/Kali preferred).
* `sudo` / Root privileges.
* A free account at [Ngrok.com](https://ngrok.com).

## 🗑️ Cleanup (Uninstall)

To stop the tunnel and remove the script:

```bash
pkill ngrok
rm setup.sh
# To restore original SSH config (optional):
# sudo cp /etc/ssh/sshd_config.bak /etc/ssh/sshd_config
# sudo service ssh restart

```

EOF
