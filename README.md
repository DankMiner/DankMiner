

<h1 align="center">DankMiner v1.2.7</h1>

<p align="center">
  <strong>High-performance GPU miner for CapStash (CAP)</strong><br>
  WXRF Proof-of-Work &bull; Pool + Solo Mining &bull; Windows &bull; Linux &bull; HiveOS
</p>

<p align="center">
  <a href="https://1miner.net"><img src="https://img.shields.io/badge/pool-1Miner.net-00ccff?style=for-the-badge" alt="1Miner.net"></a>
  <a href="#"><img src="https://img.shields.io/badge/algo-whirlpool--xorfold-0088cc?style=for-the-badge" alt="Algorithm"></a>
  <a href="#"><img src="https://img.shields.io/badge/version-1.2.7-00aaff?style=for-the-badge" alt="Version"></a>
  <a href="#"><img src="https://img.shields.io/badge/fee-5%25-005588?style=for-the-badge" alt="Dev Fee"></a>
</p>

<p align="center">
  <a href="#"><img src="https://img.shields.io/badge/Windows-0078D6?style=flat-square&logo=windows&logoColor=white" alt="Windows"></a>
  <a href="#"><img src="https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black" alt="Linux"></a>
  <a href="#"><img src="https://img.shields.io/badge/HiveOS-FFB800?style=flat-square" alt="HiveOS"></a>
  <a href="#"><img src="https://img.shields.io/badge/NVIDIA-76B900?style=flat-square&logo=nvidia&logoColor=white" alt="NVIDIA"></a>
  <a href="#"><img src="https://img.shields.io/badge/AMD-ED1C24?style=flat-square&logo=amd&logoColor=white" alt="AMD"></a>
</p>

---

## Downloads

| Platform | Download |
|----------|----------|
| **Windows** (GUI + CLI) | [DankMinerV1.2.7.zip](../../releases/download/DankMinerV1.2.7/DankMinerV1.2.7.zip) |
| **Linux / HiveOS** | [DankMiner-v1.2.7-linux.tar.gz](../../releases/download/DankMinerV1.2.7/DankMiner-v1.2.7-linux.tar.gz) |

---

## Overview

DankMiner is a GPU-accelerated miner for the **CapStash** blockchain, implementing the Whirlpool XOR-Fold proof-of-work algorithm. Built for maximum hashrate with OpenCL and CUDA support.

### Features

- **Pool Mining** — Stratum protocol (Bitcoin-style, MiningCore compatible)
- **Solo Mining** — Direct to node via getblocktemplate RPC
- **Windows GUI** — 1-click mining with pool/solo toggle
- **CLI Mode** — Headless operation for rigs and HiveOS
- **Multi-GPU** — Automatic detection and parallel dispatch
- **NVIDIA + AMD** — OpenCL for all GPUs, native CUDA for NVIDIA
- **Wallet Rotation** — Rotate payout addresses on a timer
- **Optimized Kernel** — Precomputed key schedule, local memory T0 table, multi-nonce per thread

---

## Quick Start — Windows

1. Download [DankMinerV1.2.7.zip](../../releases/download/DankMinerV1.2.7/DankMinerV1.2.7.zip)
2. Extract and run `DankMiner.exe`
3. Select **Pool Mining**, enter your wallet address
4. Click **Start Mining**

Pool is pre-configured to `stratum+tcp://1miner.net:3691`.

**CLI:**
```
DankMiner.exe --cli --pool stratum+tcp://1miner.net:3691 --address YOUR_ADDRESS
```

---

## Quick Start — Linux

```bash
tar xzf DankMiner-v1.2.7-linux.tar.gz
cd dankminer
./install.sh                # install pyopencl, numpy, pycuda
nano mine.sh                # set your wallet address
./mine.sh                   # start mining
```

**Solo mining:**
```bash
nano mine_solo.sh           # set your node credentials + address
./mine_solo.sh
```

**CLI directly:**
```bash
python3 dankminer.py --cli --pool stratum+tcp://1miner.net:3691 --address YOUR_ADDRESS
```

---

## Quick Start — HiveOS

**Step 1 — Upload to rig:**
```bash
scp DankMiner-v1.2.7-linux.tar.gz root@YOUR_RIG_IP:/tmp/
ssh root@YOUR_RIG_IP
mkdir -p /hive/miners/custom/dankminer
tar xzf /tmp/DankMiner-v1.2.7-linux.tar.gz -C /hive/miners/custom/
```

**Step 2 — Create Flight Sheet:**

| Field | Value |
|-------|-------|
| Miner | Custom |
| Pool URL | `stratum+tcp://1miner.net:3691` |
| Template | `YOUR_CAP_ADDRESS` |
| Worker | `%WORKER_NAME%` |

The miner reports hashrate, GPU temps, fan speeds, and share counts to the HiveOS dashboard automatically.

---

## Pool Mining

| Setting | Value |
|---------|-------|
| Pool | `stratum+tcp://1miner.net:3691` |
| Algorithm | `whirlpool-xorfold` |
| Coin | CapStash (CAP) |

### Supported Pools

- [1Miner.net](https://1miner.net) — Official CapStash pool

---

## Performance

| GPU | Hashrate |
|-----|----------|
| RTX 4090 | ~2.0+ GH/s |
| RTX 4070 SUPER | ~1.0–1.2 GH/s |
| RTX 3070 | ~500 MH/s |
| RX 7900 XTX | ~800 MH/s |
| RX 570 | ~80 MH/s |

> Actual hashrates depend on drivers, system configuration, and pool difficulty.

---

## CLI Options

```
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

---

## Requirements

**Windows:**
- Windows 10/11 (64-bit)
- NVIDIA or AMD GPU with latest drivers
- No other dependencies — everything is bundled in the .exe

**Linux / HiveOS:**
- Python 3.8+
- pyopencl + numpy (installed via `./install.sh`)
- pycuda (optional, auto-installed for NVIDIA)
- GPU with OpenCL 1.2+ or CUDA support

---

## v1.2.7 Changelog

**Performance**
- Removed per-share CPU verification bottleneck
- Throttled network polling to reduce per-batch overhead
- Batched stats and submit processing
- Significantly faster stratum hashrate vs previous versions

**Features**
- Pool + Solo mining with GUI toggle (Windows)
- Stratum protocol support (MiningCore / Bitcoin-style)
- Shares accepted/rejected counter
- Pool URL and worker name with config persistence
- HiveOS integration with full dashboard stats (hashrate, temps, fans, shares)
- Linux standalone scripts with auto-installer
- Wallet rotation support

---

## Dev Fee

5% dev fee to support continued development. The miner mines to the developer address for a short period every 30 minutes after a 5-minute warmup.

---

## Disclaimer

Use at your own risk. Mining cryptocurrency consumes electricity and generates heat. Ensure adequate cooling and monitor your hardware. The developers are not responsible for any hardware damage, financial loss, or other issues arising from the use of this software.

---

## Links

- **Pool**: [1Miner.net](https://1miner.net)

---

<p align="center">
  <sub>(c) 2026 DankMiner / <a href="https://1miner.net">1Miner.net</a></sub>
</p>
