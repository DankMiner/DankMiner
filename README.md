# DankMiner v1.2.9a
**GPU Miner for CapStash & Xelis**
Works on NVIDIA and AMD GPUs. Pool and solo mining. HiveOS ready.

---

## Downloads

| Platform | Download | GPUs |
|----------|----------|------|
| **Windows** | [DankMiner-v1.2.9a-Windows.zip](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.2.9a/DankMiner-v1.2.9a-Windows.zip) | NVIDIA + AMD |
| **Linux** | [DankMiner-v1.2.9a-Linux.tar.gz](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.2.9a/DankMiner-v1.2.9a-Linux.tar.gz) | NVIDIA + AMD |
| **HiveOS** | [dankminer-1.2.9a-hiveos.tar.gz](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.2.9a/dankminer-1.2.9a-hiveos.tar.gz) | NVIDIA + AMD |
| **Linux RTX 50 Series** | [dankminer-1.2.9a-rtx50.tar.gz](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.2.9a/dankminer-1.2.9a-rtx50.tar.gz) | RTX 5060–5090 + all others |

> **Which do I need?**
> - **Windows** → `DankMiner-v1.2.9a-Windows.zip`
> - **Linux desktop** → `DankMiner-v1.2.9a-Linux.tar.gz`
> - **HiveOS** → `dankminer-1.2.9a-hiveos.tar.gz`
> - **RTX 5060/5070/5080/5090 on Linux** → `dankminer-1.2.9a-rtx50.tar.gz`

---

## Quick Start

**Pool Mining:**
```
dankminer -a capstash -w YOUR_ADDRESS -p stratum+tcp://1miner.net:3691 --worker rig1
```

**Solo Mining:**
```
dankminer -a capstash -w YOUR_ADDRESS -p http://user:pass@127.0.0.1:33333
```

**Xelis:**
```
dankminer -a xelis -w YOUR_ADDRESS -p stratum+tcp://1miner.net:3400
```

---

## Options

```
  -a ALGO          Algorithm: capstash, xelis
  -w WALLET        Wallet address
  -p URL           Pool or RPC URL
  -W NAME          Worker name
  -wl FILE         Wallet list for rotation
  --force-opencl   Force OpenCL (for AMD GPUs if auto-detect doesn't work)
```

---

## Supported GPUs

**NVIDIA:** GTX 1060 through RTX 5090
**AMD:** RX 470/480/570/580, Vega, RX 5000/6000/7000/9000 series

AMD GPUs are auto-detected — no extra config needed.

---

## Web Dashboard

Open `http://localhost:4068` in your browser while mining. Shows live hashrate, accepted/rejected shares, GPU temperature, fan speed, and power draw.

---

## Stratum Servers

### Pool (PPLNS)

| Port | Type | Difficulty |
|------|------|------------|
| **3690** | CPU | VarDiff 0.01 (0.0005 → 1) |
| **3691** | Industrial | VarDiff 1 (0.01 → ∞) |

### Solo

| Port | Type | Difficulty |
|------|------|------------|
| **3790** | CPU | VarDiff 0.01 (0.0005 → 1) |
| **3791** | Industrial | VarDiff 1 (0.01 → ∞) |

### Server Regions

| Region | Hostname |
|--------|----------|
| US-TX (North America) | `1miner.net` |
| EU-FR (Europe) | `eu1.1miner.net` |
| SGP (Singapore) | `sgp.1miner.net` |

**Examples:**
```
stratum+tcp://1miner.net:3691          # US pool
stratum+tcp://eu1.1miner.net:3691      # EU pool
stratum+tcp://sgp.1miner.net:3691      # Singapore pool
stratum+tcp://1miner.net:3791          # US solo
```

**Xelis:**
- USA: `stratum+tcp://1miner.net:3400`

---

## HiveOS

| Field | Value |
|-------|-------|
| Miner name | `dankminer` |
| Installation URL | `https://1miner.net/miners/dankminer-1.2.9a.tar.gz` |
| Hash algorithm | `whirlpool` |
| Wallet | Your CapStash address |
| Pool URL | `stratum+tcp://1miner.net:3691` |

Use the server closest to you: `1miner.net` (US), `eu1.1miner.net` (EU), or `sgp.1miner.net` (Singapore).

RTX 50 series: use `dankminer-1.2.9a-rtx50.tar.gz` instead.

---

## Dev Fee

| Algorithm | Fee |
|-----------|-----|
| CapStash | 2% |
| Xelis | 1% |

---

## Troubleshooting

**"CUDA err 999 / unknown error"** — GPU driver reset. v1.2.9a recovers automatically. If it happens frequently on Windows, run `install_tdr_fix.bat` as Administrator and reboot to raise the Windows GPU watchdog timeout.

**"no kernel image available"** — Wrong build for your GPU. RTX 50 series needs the RTX 50 build. GTX 10 series needs the standard Linux build.

**"CUDA not available — trying OpenCL"** — Normal on AMD rigs. No action needed.

**"GLIBC not found"** — Use the HiveOS build instead.

**Low hashrate** — Check GPU temps, riser cables, and power delivery. Whirlpool is core-heavy — boost core clock, drop memory clock.

**"GPU has fallen off the bus"** — Hardware issue. Power cycle the rig. If it repeats, replace the riser on that GPU slot.

---

## What's New in v1.2.9a

- Fixed CUDA error 999 crash after sustained mining (kernel buffer overflow)
- Automatic GPU recovery from Windows TDR / driver resets
- ~10% CapStash hashrate improvement (Whirlpool V10.5 kernel)
- NVML GPU monitoring (temp, fan, power) on web dashboard
- Included `install_tdr_fix.bat` for Windows GPU timeout optimization

---

## Links

- **Pool:** [1miner.net](https://1miner.net)
- **Discord:** [discord.gg/YZXGEa9RhK](https://discord.gg/YZXGEa9RhK)
- **CapStash Core:** [github.com/CapStash/CapStash-Core](https://github.com/CapStash/CapStash-Core)

---

(c) 2026 DankMiner / 1Miner.net
