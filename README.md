# DankMiner v1.5.3
**GPU + CPU Miner for CapStash, Xelis, Warthog, Monero (RandomX) & Cereblix (NeuroMorph)**
Works on NVIDIA and AMD GPUs. CPU mining for Monero/RandomX (rx/0) and Cereblix/NeuroMorph (nm/1). Pool and solo. HiveOS ready. Dual GPU+CPU mining in one process.

---

## Downloads

| Platform | Download | Hardware |
|----------|----------|----------|
| **Windows** | [DankMiner-v1.5.3-Windows.zip](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.5.3/DankMiner-v1.5.3-Windows.zip) | NVIDIA + AMD + CPU |
| **Linux** | [DankMiner-v1.5.3-Linux.tar.gz](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.5.3/DankMiner-v1.5.3-Linux.tar.gz) | NVIDIA + AMD + CPU |
| **HiveOS** (all GPUs) | [dankminer-1.5.3.tar.gz](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.5.3/dankminer-1.5.3.tar.gz) | NVIDIA (incl. RTX 50) + AMD + CPU |

> **New in v1.5.3:** Cereblix (CRB) / NeuroMorph on the CPU — the second CPU coin
> alongside Monero/RandomX, with full GPU+CPU dual-mining support.

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

**Cereblix / CRB (CPU only, NeuroMorph nm/1):**

```
dankminer -a crb -w YOUR_CRB_ADDRESS -p stratum+tcp://us1.1miner.net:9836 --cpu-threads 14 --crb-lanes 4
```

> `-p` is optional for CRB — it defaults to `stratum+tcp://us1.1miner.net:9836`.

**Solo CapStash (against your own capstashd RPC):**

```
dankminer -a capstash -w YOUR_ADDRESS -p http://user:pass@127.0.0.1:33333
```

---

## Dual Mining (GPU + CPU at the same time)

Add `--xmr-wallet ADDR` **or** `--crb-wallet ADDR` to **any GPU primary command** to
mine a CPU coin at the same time. One process, one console, one dashboard.

**CapStash (GPU) + Cereblix (CPU):**

```
dankminer -a capstash -w YOUR_CAP_ADDRESS -p stratum+tcp://1miner.net:3691 \
          --crb-wallet YOUR_CRB_ADDRESS --crb-threads 12
```

**CapStash (GPU) + Monero (CPU):**

```
dankminer -a capstash -w YOUR_CAP_ADDRESS -p stratum+tcp://1miner.net:3691 \
          --xmr-wallet YOUR_XMR_ADDRESS --xmr-threads 12
```

- **GPU mines CapStash / Xelis / Warthog** as the primary algorithm
- **CPU mines RandomX (rx/0) or NeuroMorph (nm/1)** as a secondary passenger thread
- Each algo keeps its own dev-fee schedule independently
- Dashboard shows both side by side; console prefixes CPU lines with `[XMR]` / `[CRB]`

`--xmr-pool` accepts **any rx/0 stratum URL** (Monero, Zephyr, Salvium, Townforge, …).
`--crb-pool` accepts **any nm/1 (NeuroMorph) stratum URL** — Cereblix is currently the
only NeuroMorph coin.

**Threading rule of thumb:** both RandomX and NeuroMorph scale by *physical cores*, not
logical threads. On a 12-core / 24-thread CPU, `--crb-threads 11`/`12` beats `23`. For
NeuroMorph, also try a few `--crb-lanes` values (1–16) and keep whatever gives the most
H/s — big-cache CPUs like 4–8, cache-tight desktops like 1–2.

---

## Options

```
  -a ALGO          Algorithm: capstash, xelis, warthog (GPU); xmr, crb (CPU)
  -w WALLET        Wallet/payout address for the primary algo
  -p URL           Pool or RPC URL for the primary algo
  -W NAME          Worker name
  -wl FILE         Wallet list for rotation
  --force-opencl   Force OpenCL (for AMD GPUs if auto-detect doesn't work)
  --no-cuda        Skip CUDA entirely — OpenCL drives every GPU
  --no-ocl         Skip OpenCL entirely — CUDA only
  --cpu-threads N  CPU worker threads (for standalone xmr / crb)
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

  --crb-wallet ADDR    Run NeuroMorph nm/1 on the CPU alongside the primary
  --crb-pool URL       CRB pool URL (default stratum+tcp://us1.1miner.net:9836)
  --crb-worker NAME    Worker name for CRB (default: same as -W)
  --crb-threads N      CPU threads for CRB (default: hw cores - 1)
  --crb-lanes N        K-lane batch per worker, 1-16 (default 4)
```

---

## Supported Hardware

**NVIDIA:** GTX 1060 through RTX 5090 (CUDA, embedded fatbin)
**AMD:** RX 470/480/570/580, Vega, RX 5000/6000/7000/9000 series (OpenCL)
**Intel:** Arc (OpenCL, experimental)
**CPU (XMR):** Any x86-64 with AES-NI for RandomX
**CPU (CRB):** x86-64 with **AVX2 + AES-NI** — Intel Haswell (2013)+ / AMD Zen+

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

### Cereblix / CRB (NeuroMorph nm/1)

| Host:Port | Type |
|-----------|------|
| **us1.1miner.net:9836** | Pool (PPLNS) |

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
stratum+tcp://us1.1miner.net:9836      # CRB / NeuroMorph
```

---

## HiveOS

DankMiner runs as a **custom miner** on HiveOS. From the flight sheet → Miner → "More miners…" → **Setup miner config** opens this dialog:

![HiveOS Flight Sheet Configuration](docs/flightsheet.png)

| Field | Value |
|-------|-------|
| **Miner name** | `dankminer` (just the name — not the version) |
| **Installation URL** | `https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.5.3/dankminer-1.5.3.tar.gz` |
| **Hash algorithm** | `whirlpool` |
| **Wallet and worker template** | your CapStash address |
| **Pool URL** | `stratum+tcp://1miner.net:3691` |
| **Pass** | leave blank |
| **Extra config arguments** | leave blank for solo CapStash, or see below for dual mining |

> **Important:** the miner name must be exactly `dankminer` — not `dankminerV1.5.3` or anything else. The miner will fail to launch if the name doesn't match what's inside the tarball.

**Dual mining on HiveOS** — add the CPU-coin flags to the **Extra config arguments** field:

```
--crb-wallet YOUR_CRB_ADDRESS --crb-threads 12
```
or, for Monero instead of Cereblix:
```
--xmr-wallet YOUR_XMR_ADDRESS --xmr-threads 12
```

The same single tarball (`dankminer-1.5.3.tar.gz`) covers every supported GPU including RTX 50. Option C unified build — no separate rtx50 download.

Use the server closest to you: `1miner.net` (US), `eu1.1miner.net` (EU), or `sgp.1miner.net` (Singapore).

---

## Dev Fee

| Algorithm | Fee |
|-----------|-----|
| CapStash | 2% |
| Xelis | 1% |
| Warthog | 1% |
| XMR (RandomX) | 2% |
| CRB (NeuroMorph) | 1% |

In dual mining each algo runs its own dev-fee schedule independently — they don't both go to dev fee at the same time.

---

## Troubleshooting

**"CUDA err 999 / unknown error"** — GPU driver reset. DankMiner recovers automatically. If it happens frequently on Windows, run `install_tdr_fix.bat` as Administrator and reboot to raise the Windows GPU watchdog timeout.

**"no kernel image available"** — Your driver is older than the embedded fatbin needs. Update your NVIDIA driver to 525+ (CUDA 11.8 driver floor).

**"CUDA not available — trying OpenCL"** — Normal on AMD-only rigs. No action needed.

**"GLIBC not found"** — Use the HiveOS build (`dankminer-1.5.3.tar.gz`) instead of the Linux desktop build.

**Low GPU hashrate** — Check temps, riser cables, and power delivery. Whirlpool is core-heavy — boost core clock, drop memory clock.

**Low XMR / RandomX hashrate** — RandomX hates SMT and oversubscription. Use `--xmr-threads N` where N ≈ physical cores. Enable hugepages (`cat /proc/sys/vm/nr_hugepages` should be > 0) — RandomX loses 30-50% without them.

**Low CRB / NeuroMorph hashrate** — NeuroMorph is memory-latency bound. Use `--crb-threads N` near your physical core count, and sweep `--crb-lanes` (1–16): big-cache CPUs (EPYC/Threadripper) usually want 4–8, cache-tight desktops 1–2. Keep whatever gives the most H/s.

**"CRB needs AVX2 + AES-NI" / illegal instruction on CRB** — Your CPU is older than Haswell (2013) / Zen. CapStash/Xelis/Warthog/XMR still work; NeuroMorph does not.

**"GPU has fallen off the bus"** — Hardware issue. Power cycle the rig. If it repeats, replace the riser on that GPU slot.

---

## What's New in v1.5.3

- **Cereblix / CRB support** via the vendored NeuroMorph fast core (Cereblix's own `unm`, MIT) — byte-identical to consensus, validated against the live pool
- **CRB dual mining** — GPU primary + NeuroMorph nm/1 on the CPU in one process, one dashboard, alongside the existing XMR passenger
- **K-lane batch hashing** (`--crb-lanes`) to hide the NeuroMorph 64 MiB dataset latency
- **Default CRB pool** `stratum+tcp://us1.1miner.net:9836` — used automatically when `-p` is omitted
- **New launchers** — `mine_crb.bat` and `mine_dual_crb_capstash.bat`
- New flags: `--crb-wallet`, `--crb-pool`, `--crb-worker`, `--crb-threads`, `--crb-lanes`

### Carrying forward from v1.5.x

- **Monero / XMR (RandomX rx/0)** CPU mining + GPU/CPU dual mining, any rx/0 coin
- **Three GPU algorithms in one binary** — CapStash, Xelis, Warthog
- **Multi-GPU** on all GPU algorithms; **Hybrid CUDA + OpenCL** on CapStash
- **Per-GPU backend selection** — `--cuda=0,1`, `--ocl=0`, `--no-cuda`, `--no-ocl`
- **CPU control flags** — `--cpu-cores`, `--cpu-affinity`, plus matching `config.txt`
- **Redesigned dashboard** with side-by-side dual-mining cards and CPU model detection
- **Single unified Linux tarball** — CUDA 12.8 fatbins (native sm_120 for RTX 50) linked under CUDA 11.8 (driver floor 525+)

---

## Links

- **Pool:** [1miner.net](https://1miner.net)
- **Discord:** [discord.gg/YZXGEa9RhK](https://discord.gg/YZXGEa9RhK)
- **Cereblix:** [github.com/CereblixCRB/cereblix](https://github.com/CereblixCRB/cereblix)
- **CapStash Core:** [github.com/CapStash/CapStash-Core](https://github.com/CapStash/CapStash-Core)

---

(c) 2026 DankMiner / 1Miner.net
