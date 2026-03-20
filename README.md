<p align="center">
  <img src="https://raw.githubusercontent.com/1minerNet/DankMiner/main/assets/banner.png" alt="DankMiner" width="600">
</p>

<h1 align="center">DankMiner v1.2.7</h1>

<p align="center">
  <strong>High-performance GPU miner for CapStash (CAP)</strong><br>
  Whirlpool XOR-Fold Proof-of-Work &bull; Pool + Solo Mining &bull; Windows GUI &amp; CLI
</p>

<p align="center">
  <a href="https://1miner.net"><img src="https://img.shields.io/badge/pool-1Miner.net-00ccff?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZD0iTTEyIDJMMiAyMmgyMEwxMiAyeiIgZmlsbD0iIzAwY2NmZiIvPjwvc3ZnPg==" alt="1Miner.net"></a>
  <a href="#"><img src="https://img.shields.io/badge/algo-whirlpool--xorfold-0088cc?style=for-the-badge" alt="Algorithm"></a>
  <a href="#"><img src="https://img.shields.io/badge/version-1.2.7-00aaff?style=for-the-badge" alt="Version"></a>
  <a href="#"><img src="https://img.shields.io/badge/platform-Windows-0078D6?style=for-the-badge&logo=windows" alt="Windows"></a>
  <a href="#"><img src="https://img.shields.io/badge/GPU-NVIDIA%20%7C%20AMD-76b900?style=for-the-badge" alt="GPU Support"></a>
</p>

---

## Overview

DankMiner is a GPU-accelerated miner for the **CapStash** blockchain, implementing the Whirlpool XOR-Fold proof-of-work algorithm. Built for maximum hashrate with OpenCL and CUDA support.

### Key Features

- **Pool Mining** — Stratum protocol (Bitcoin-style, MiningCore compatible)
- **Solo Mining** — Direct to node via getblocktemplate RPC
- **Windows GUI** — 1-click mining with pool/solo toggle
- **CLI Mode** — Headless operation for rigs and HiveOS
- **Multi-GPU** — Automatic detection and parallel dispatch
- **NVIDIA + AMD** — OpenCL for all GPUs, native CUDA for NVIDIA
- **Wallet Rotation** — Rotate payout addresses on a timer
- **Optimized Kernel** — Precomputed key schedule, local memory T0 table, multi-nonce per thread

## Quick Start

### GUI (Windows)

1. Download `DankMiner.exe` from [Releases](../../releases)
2. Double-click to launch
3. Select **Pool Mining** or **Solo Mining**
4. Enter your CapStash wallet address
5. Click **Start Mining**

The default pool is pre-configured to `stratum+tcp://1miner.net:3691`.

### CLI (Windows / Linux)

**Pool mining:**
```bash
DankMiner.exe --cli --pool stratum+tcp://1miner.net:3691 --address YOUR_CAP_ADDRESS
```

**Solo mining:**
```bash
DankMiner.exe --cli --host 127.0.0.1 --port 33333 --user capstashrpc --password YOUR_PASS --address YOUR_CAP_ADDRESS
```

## Pool Mining

DankMiner connects to stratum-compatible mining pools using the standard Bitcoin mining protocol.

| Setting | Value |
|---------|-------|
| Pool | `stratum+tcp://1miner.net:3691` |
| Algorithm | `whirlpool-xorfold` |
| Coin | CapStash (CAP) |

### Supported Pools

- [1Miner.net](https://1miner.net) — Official CapStash pool

## Performance

Hashrates vary by GPU. Approximate values on the Whirlpool XOR-Fold algorithm:

| GPU | Hashrate |
|-----|----------|
| RTX 4070 SUPER | ~1.0–1.2 GH/s |
| RTX 4090 | ~2.0+ GH/s |
| RX 7900 XTX | ~800 MH/s |
| RTX 3070 | ~500 MH/s |
| RX 570 | ~80 MH/s |

> Actual hashrates depend on driver version, system configuration, and pool difficulty.

## CLI Options

```
Options:
  --pool URL           Stratum pool URL (stratum+tcp://host:port)
  --host HOST          RPC host for solo mining (default: 127.0.0.1)
  --port PORT          RPC port for solo mining (default: 33333)
  --user USER          RPC username
  --password PASS      RPC password
  --address ADDR       Mining/payout address (required)
  --wallet-list FILE   Wallet rotation file (one address per line)
  --device INDEX       GPU index: 0, 1, 2... or "all" (default: all)
  --no-gpu             Force CPU mining (slow, for testing only)
```

## HiveOS

DankMiner supports HiveOS as a custom miner.

**Flight Sheet Configuration:**

| Field | Value |
|-------|-------|
| Miner | Custom |
| Installation URL | *(upload tarball to rig)* |
| Pool URL | `stratum+tcp://1miner.net:3691` |
| Wallet | Your CAP address |
| Worker | `%WORKER_NAME%` |

## Requirements

- **GPU**: NVIDIA (OpenCL or CUDA) or AMD (OpenCL)
- **OS**: Windows 10/11 (64-bit)
- **Drivers**: Latest NVIDIA or AMD GPU drivers
- **VRAM**: 2GB+ recommended

The standalone `.exe` has no additional dependencies — Python, OpenCL libraries, and the mining kernel are all bundled.

## v1.2.7 Changelog

**Performance**
- Removed per-share CPU verification bottleneck — GPU stays saturated
- Throttled network polling to reduce per-batch overhead
- Batched stats and submit result processing
- Up to 4-5x faster stratum hashrate vs v1.2.5

**Features**
- Pool + Solo mining in one binary with GUI toggle
- Stratum protocol support (MiningCore / Bitcoin-style)
- Shares accepted/rejected counter in GUI
- Pool URL and worker name in GUI with config persistence
- Wallet rotation support (`--wallet-list`)

**Fixes**
- Fixed hashrate/stats attribute errors in pool mode
- Fixed GUI stats callback for stratum mode
- Proper version stamping across all components

## Dev Fee

DankMiner includes a 5% dev fee to support continued development. The miner mines to the developer address for 90 seconds every 30 minutes (after a 5-minute warmup). This applies to both pool and solo mining.

## Disclaimer

Use at your own risk. Mining cryptocurrency consumes electricity and generates heat. Ensure adequate cooling and monitor your hardware. The developers are not responsible for any hardware damage, financial loss, or other issues arising from the use of this software.

## Links

- **Pool**: [1Miner.net](https://1miner.net)
- **CapStash**: [CapStash Explorer](https://1miner.net)

---

<p align="center">
  <sub>(c) 2026 DankMiner / <a href="https://1miner.net">1Miner.net</a></sub>
</p>
