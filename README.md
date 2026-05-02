# DankMiner v1.4.0
**GPU + CPU Miner for CapStash, Xelis, Warthog & Monero (RandomX)**
Works on NVIDIA and AMD GPUs. CPU mining for any rx/0 RandomX coin. Pool and solo. HiveOS ready. Dual GPU+CPU mining in one process.

---

## Downloads

| Platform | Download | Hardware |
|----------|----------|----------|
| **Windows** | [DankMiner-v1.4.0-Windows.zip](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.4.0/DankMiner-v1.4.0-Windows.zip) | NVIDIA + AMD + CPU |
| **Linux** | [DankMiner-v1.4.0-Linux.tar.gz](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.4.0/DankMiner-v1.4.0-Linux.tar.gz) | NVIDIA + AMD + CPU |
| **HiveOS** (all GPUs) | [dankminer-1.4.0.tar.gz](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.4.0/dankminer-1.4.0.tar.gz) | NVIDIA (incl. RTX 50) + AMD + CPU |

> **Heads-up for v1.3.0a users:** v1.4.0 ships a **single unified Linux tarball** that replaces the old compat + rtx50 split. The new build has CUDA 12.8 fatbins (native sm_120 SASS for RTX 50) but stays linked under CUDA 11.8 so the driver floor remains 525+ — same broad HiveOS compat as the old compat tarball, plus full RTX 50 support, in one file.

---

## Web Dashboard

Open `http://localhost:4068` in your browser while mining. Live hashrate per algorithm, per-GPU shares, GPU temp / fan / power, and the actual CPU model when CPU mining is active. Side-by-side primary/secondary cards in dual mining mode.

![DankMiner Dashboard](docs/dashboard.png)

---

## Quick Start

**CapStash (GPU):**
```
dankminer -a capstash -w YOUR_ADDRESS -p stratum+tcp://1miner.net:3691 --worker rig1
```

**Xelis (GPU):**
```
dankminer -a xelis -w YOUR_ADDRESS -p stratum+tcp://1miner.net:4073
```

**Warthog (GPU + CPU, JanusHash):**
```
dankminer -a warthog -w YOUR_ADDRESS -p stratum+tcp://1miner.net:4200
```

**Monero / XMR (CPU only, RandomX rx/0):**
```
dankminer -a xmr -w YOUR_XMR_ADDRESS -p stratum+tcp://1miner.net:3333
```

**Solo CapStash (against your own capstashd RPC):**
```
dankminer -a capstash -w YOUR_ADDRESS -p http://user:pass@127.0.0.1:33333
```

---

## Dual Mining (GPU + CPU at the same time)

Add `--xmr-wallet ADDR` to **any GPU primary command** to mine RandomX rx/0 on the CPU at the same time. One process, one console, one dashboard.

```
dankminer -a capstash -w YOUR_CAP_ADDRESS -p stratum+tcp://1miner.net:3691 \
          --xmr-wallet YOUR_XMR_ADDRESS --xmr-threads 12
```

- **GPU mines CapStash / Xelis / Warthog** as the primary algorithm
- **CPU mines RandomX rx/0** as a secondary passenger thread
- Each algo keeps its own dev-fee schedule independently
- Dashboard shows both side by side; console prefixes XMR lines with `[XMR]`

`--xmr-pool` accepts **any rx/0 stratum URL** — Monero, Zephyr, Salvium, Townforge, and any other coin using standard RandomX rx/0 parameters. Variants with non-default RandomX parameters (Wownero rx/wow, ArQmA rx/arq) are not supported.

**Custom rx/0 pool example:**
```
dankminer -a capstash -w YOUR_CAP_ADDRESS -p stratum+tcp://1miner.net:3691 \
          --xmr-wallet YOUR_RX_ADDRESS \
          --xmr-pool stratum+tcp://your-rx0-pool.example:3333 \
          --xmr-threads 12
```

**Threading rule of thumb:** RandomX scales by *physical cores*, not logical threads. On a 12-core / 24-thread CPU, `--xmr-threads 11` or `12` will outperform `--xmr-threads 23`. Less is more.

---

## Options

```
  -a ALGO          Algorithm: capstash, xelis, warthog, xmr
  -w WALLET        Wallet/payout address for the primary algo
  -p URL           Pool or RPC URL for the primary algo
  -W NAME          Worker name
  -wl FILE         Wallet list for rotation
  --force-opencl   Force OpenCL (for AMD GPUs if auto-detect doesn't work)
  --no-cuda        Skip CUDA entirely — OpenCL drives every GPU
  --no-ocl         Skip OpenCL entirely — CUDA only
  --cpu-cores N    Pin the miner to the first N logical CPU cores
  --cpu-affinity LIST   Pin process to specific cores (e.g. 0,1 or 0-3)
```

**Dual-mining flags** (only valid with a GPU primary):
```
  --xmr-wallet ADDR    Run RandomX rx/0 on the CPU alongside the primary
  --xmr-pool URL       RandomX pool URL (default stratum+tcp://1miner.net:3333)
  --xmr-worker NAME    Worker name for XMR (default: same as -W)
  --xmr-threads N      CPU threads for XMR (default: hw cores - 1)
  --xmr-light          Light-mode RandomX (256 MiB, ~10x slower than full)
```

---

## Supported Hardware

**NVIDIA:** GTX 1060 through RTX 5090 (CUDA, embedded fatbin)
**AMD:** RX 470/480/570/580, Vega, RX 5000/6000/7000/9000 series (OpenCL)
**Intel:** Arc (OpenCL, experimental)
**CPU:** Any x86-64 with AES-NI for RandomX (XMR / dual mining)

AMD GPUs are auto-detected — no extra config needed. Hybrid NVIDIA + AMD rigs run in the same instance with each card on its native backend.

---

## Stratum Servers

### CapStash Pool (PPLNS)

| Port | Type | Difficulty |
|------|------|------------|
| **3690** | CPU | VarDiff 0.01 (0.0005 → 1) |
| **3691** | Industrial | VarDiff 1 (0.01 → ∞) |

### CapStash Solo

| Port | Type | Difficulty |
|------|------|------------|
| **3790** | CPU | VarDiff 0.01 (0.0005 → 1) |
| **3791** | Industrial | VarDiff 1 (0.01 → ∞) |

### Xelis

| Port | Type |
|------|------|
| **4073** | Pool (PPLNS) |
| **4074** | Solo |

### Warthog

| Port | Type |
|------|------|
| **4200** | Pool (PPLNS) |
| **4201** | Solo |

### Monero / XMR (RandomX rx/0)

| Port | Type |
|------|------|
| **3333** | Pool (PPLNS) |

### Server Regions

| Region | Hostname |
|--------|----------|
| US-TX (North America) | `1miner.net` |
| EU-FR (Europe) | `eu1.1miner.net` |
| SGP (Singapore) | `sgp.1miner.net` |

**Examples:**
```
stratum+tcp://1miner.net:3691          # CapStash US pool
stratum+tcp://eu1.1miner.net:3691      # CapStash EU pool
stratum+tcp://sgp.1miner.net:3691      # CapStash Singapore pool
stratum+tcp://1miner.net:3791          # CapStash US solo
stratum+tcp://1miner.net:4073          # Xelis
stratum+tcp://1miner.net:4200          # Warthog
stratum+tcp://1miner.net:3333          # XMR / RandomX
```

---

## HiveOS

DankMiner runs as a **custom miner** on HiveOS. From the flight sheet → Miner → "More miners…" → **Setup miner config** opens this dialog:

![HiveOS Flight Sheet Configuration](docs/flightsheet.png)

| Field | Value |
|-------|-------|
| **Miner name** | `dankminer` (just the name — not the version) |
| **Installation URL** | `https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.4.0/dankminer-1.4.0.tar.gz` |
| **Hash algorithm** | `whirlpool` |
| **Wallet and worker template** | your CapStash address |
| **Pool URL** | `stratum+tcp://1miner.net:3691` |
| **Pass** | leave blank |
| **Extra config arguments** | leave blank for solo CapStash, or see below for dual mining |

> **Important:** the miner name must be exactly `dankminer` — not `dankminerV1.4.0` or anything else. The miner will fail to launch if the name doesn't match what's inside the tarball.

**Dual mining on HiveOS** — add the XMR flags to the **Extra config arguments** field:
```
--xmr-wallet YOUR_XMR_ADDRESS --xmr-threads 12
```

The same single tarball (`dankminer-1.4.0.tar.gz`) covers every supported GPU including RTX 50. No separate rtx50 download anymore — Option C unified build.

Use the server closest to you: `1miner.net` (US), `eu1.1miner.net` (EU), or `sgp.1miner.net` (Singapore).

---

## Dev Fee

| Algorithm | Fee |
|-----------|-----|
| CapStash | 2% |
| Xelis | 1% |
| Warthog | 1% |
| XMR (RandomX) | 2% |

In dual mining each algo runs its own dev-fee schedule independently — they don't both go to dev fee at the same time.

---

## Troubleshooting

**"CUDA err 999 / unknown error"** — GPU driver reset. DankMiner recovers automatically. If it happens frequently on Windows, run `install_tdr_fix.bat` as Administrator and reboot to raise the Windows GPU watchdog timeout.

**"no kernel image available"** — Your driver is older than the embedded fatbin needs. Update your NVIDIA driver to 525+ (CUDA 11.8 driver floor).

**"CUDA not available — trying OpenCL"** — Normal on AMD-only rigs. No action needed.

**"GLIBC not found"** — Use the HiveOS build (`dankminer-1.4.0.tar.gz`) instead of the Linux desktop build.

**Low GPU hashrate** — Check temps, riser cables, and power delivery. Whirlpool is core-heavy — boost core clock, drop memory clock.

**Low XMR / RandomX hashrate** — RandomX hates SMT and oversubscription. Use `--xmr-threads N` where N ≈ physical cores (not logical threads). On a 12c/24t CPU try `--xmr-threads 12`, not 23. Also check that hugepages are enabled (`cat /proc/sys/vm/nr_hugepages` should be > 0) — RandomX loses 30-50% without them.

**"GPU has fallen off the bus"** — Hardware issue. Power cycle the rig. If it repeats, replace the riser on that GPU slot.

**Duplicate share rejects on big-CPU rigs** — Fixed in v1.4.0 (nonce-slice overflow on >64 worker threads). Update from any earlier dual-mining build.

---

## What's New in v1.4.0

- **Monero / XMR support** via vendored tevador/RandomX v1.2.1 — the same engine xmrig uses
- **Dual mining** — GPU primary + RandomX rx/0 on CPU in one process, one dashboard
- **Any rx/0 coin** — `--xmr-pool` is not Monero-locked; works with Zephyr, Salvium, Townforge, and any other coin using standard rx/0
- **Redesigned web dashboard** — flat black palette, embedded DankMiner logo, side-by-side dual-mining cards
- **Embedded exe icon** — Windows Explorer / taskbar / alt-tab now show the DankMiner logo
- **CPU model detection** — dashboard and console show the actual chip brand (`Ryzen 7 7800X3D`, `EPYC 9754`, `Core i9-13900K`) instead of a thread-count placeholder
- **Single unified Linux tarball** — replaces the prior compat + rtx50 split. Same binary works on every supported GPU.
- **Bug fixes** — dual-mining HR-thread race fixed (no more KH/s flicker into the GPU hashrate slot), nonce-slice overflow fixed for >64-thread CPUs, Monero stratum target endianness corrected to xmrig-compatible LE u32/u64

### Carrying forward from v1.3.0a

- **Three GPU algorithms in one binary** — CapStash, Xelis, Warthog
- **Multi-GPU support** on all GPU algorithms
- **Hybrid CUDA + OpenCL** on CapStash for mixed-vendor rigs
- **Per-GPU stats** — every share logged with which card found it, plus per-GPU accept/reject in the dashboard
- **Per-GPU backend selection** — `--cuda=0,1`, `--ocl=0`, `--no-cuda`, `--no-ocl`
- **CPU control flags** — `--cpu-cores`, `--cpu-affinity`, plus matching `config.txt` support
- **Improved reconnect handling** and smoother multi-GPU share reporting
- **`config.txt` fields:** `worker`, `cpu_threads`, `cpu_cores`, `cpu_affinity`, device filters

---

## Links

- **Pool:** [1miner.net](https://1miner.net)
- **Discord:** [discord.gg/YZXGEa9RhK](https://discord.gg/YZXGEa9RhK)
- **CapStash Core:** [github.com/CapStash/CapStash-Core](https://github.com/CapStash/CapStash-Core)

---

(c) 2026 DankMiner / 1Miner.net
