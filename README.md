---

# 🚀 Automated NGINX Installation using Bash + Ansible

This project provides a **robust, interactive, and enterprise-ready automation** to install and validate **NGINX using the official NGINX repositories** across **RedHat and Debian based Linux distributions**.

It combines:

* 🧩 **Bash wrapper script** for interactive user & privilege selection
* ⚙️ **Ansible playbook** for OS-aware NGINX installation, validation, and service verification

---

## 📌 Features

* ✅ Supports **RedHat & Debian families**
* ✅ Uses **NGINX official repositories** (secure & production-ready)
* ✅ Interactive **user selection**
* ✅ Supports **sudo** and **dzdo (Centrify/Delinea)** privilege escalation
* ✅ GPG key verification for repository security
* ✅ Safe `index.html` backup before overwrite
* ✅ Configuration validation using `nginx -t`
* ✅ HTTP 200 verification using Ansible `uri`
* ✅ No dependency on systemd (`nginx` started via binary)

---

## 🖥 Supported Operating Systems

### 🔴 RedHat Family

* RHEL 7, 8, 9
* CentOS
* Rocky Linux
* AlmaLinux

### 🔵 Debian Family

* Ubuntu (all supported LTS versions)
* Debian

---

## 📂 Repository Structure

```text
.
├── install_nginx.sh        # Interactive Bash wrapper
├── install_nginx.yml       # Main Ansible playbook
├── index.html              # Sample web page deployed by Ansible
├── hosts                   # Ansible inventory
└── README.md               # Project documentation
```

---

## 🔧 Prerequisites

Ensure the following are available on the **control node**:

* Ansible ≥ 2.12
* SSH access to target hosts
* Valid sudo or dzdo privileges
* Internet access on target hosts (for NGINX repo)

---

## 🧠 How It Works

### Step 1: Bash Script (User Interaction)

The Bash script:

* Dynamically generates a list of users (`aduser01` … `aduser05`, `sandeep`, `ansible`)
* Prompts for:

  * **Remote SSH user**
  * **Privilege escalation method** (`sudo` or `dzdo`)
* Passes the selected become method to Ansible via `--extra-vars`

```bash
./install_nginx.sh
```

---

### Step 2: Ansible Playbook (Automation Engine)

The Ansible playbook performs:

#### 🔐 Privilege Handling

* Uses `become_method` dynamically (`sudo` / `dzdo`)
* Executes all privileged tasks as `root`

---

## 🧱 RedHat Family Workflow

1. Detects `yum` or `dnf`
2. Installs repository prerequisites
3. Adds **NGINX official yum repo**
4. Refreshes metadata safely
5. Installs NGINX
6. Validates binary availability

---

## 📦 Debian / Ubuntu Workflow

1. Installs required packages
2. Cleans old nginx repo configs
3. Downloads & converts NGINX GPG key
4. **Verifies key fingerprint**
5. Adds official NGINX APT repo
6. Installs NGINX securely

---

## 🌐 Post-Installation Validation

After installation, the playbook:

* 📄 Backs up existing `index.html` (timestamped)
* 📄 Deploys a new sample `index.html`
* 🔍 Validates config using `nginx -t`
* 🚀 Starts NGINX using binary
* ⏳ Waits for port **80**
* ✅ Confirms **HTTP 200 OK**

---

## ▶️ Usage

### 1️⃣ Make script executable

```bash
chmod +x install_nginx.sh
```

### 2️⃣ Run installer

```bash
./install_nginx.sh
```

### 3️⃣ Follow prompts

* Select SSH user
* Select `sudo` or `dzdo`
* Enter SSH and become passwords

---

## 🛡 Security Highlights

* Uses **signed official repositories**
* Verifies **NGINX GPG key fingerprints**
* No unsafe `apt-key` usage
* Idempotent & safe to re-run

---

## 🧪 Validation Output Example

```text
===== NGINX HTTP RESPONSE =====
Host: server01
HTTP/1.1 200 OK
```

---

## ⚠️ Notes & Known Considerations

* `ansible_remote_tmp` is explicitly set to `/tmp` for compatibility with AD users
* Designed for **AWX / Tower / enterprise environments**
* Works with password-based SSH (can be extended to SSH keys)

---

## 📌 Customization Ideas

* Add systemd service handling
* Enable TLS (Let’s Encrypt)
* Extend to NGINX Plus
* Add reverse proxy templates

---

## 👨‍💻 Author

**Sandeep Bandela**
Linux | Ansible | Automation | Enterprise Infrastructure

---

## 📄 License

This project is released under the **MIT License**.
Feel free to use, modify, and share.

---
