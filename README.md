# DankMiner v1.5.5

**GPU + CPU Miner for Quantus, CapStash, Xelis, Warthog, Monero and Coin**

Mine with NVIDIA CUDA, compatible dedicated OpenCL GPUs and x86-64 CPUs. Run a GPU algorithm alongside Monero or Coin in one process, one console and one dashboard. Windows, Linux and HiveOS packages are available.

> **New in v1.5.5:** Quantus / QTC GPU mining with fast and exact arithmetic modes, a Warthog OpenCL backend and startup repair, corrected Xelis GPU selection, and cleaner production packages. **CRB / Cereblix is no longer supported.**

---

## Downloads

| Platform | Download | Package |
|---|---|---|
| **Windows x64** | [DankMiner-v1.5.5-Windows.zip](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.5.5/DankMiner-v1.5.5-Windows.zip) | Miner, Coin runtime library and 14 `.bat` launchers |
| **Linux x86-64** | [DankMiner-v1.5.5-Linux.tar.gz](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.5.5/DankMiner-v1.5.5-Linux.tar.gz) | Miner and Coin runtime library; use CLI or configuration files |
| **HiveOS** | [dankminer-1.5.5.tar.gz](https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.5.5/dankminer-1.5.5.tar.gz) | Linux miner with flight-sheet configuration, start, stop and statistics integration |

Extract into a **new, empty folder**, then transfer your wallet and pool settings. Linux and HiveOS downloads contain no `mine_*` launchers. All packages include example configurations and third-party notices; implementation source archives and private mining settings are excluded.

[Full release notes and validation](https://github.com/DankMiner/DankMiner/releases/tag/DankMinerV1.5.5)

---

## Web Dashboard

Open `http://localhost:4068` while mining. The dashboard shows algorithm and device hashrates, accepted/rejected work, and available GPU temperature, fan and power readings. Dual mining displays the GPU primary and CPU companion separately. Coin reports **accepted blocks**, while pool algorithms report shares.

![DankMiner dashboard example](https://raw.githubusercontent.com/DankMiner/DankMiner/main/docs/dashboard.png)

---

## Quick Start

On Windows, edit a `mine_*.bat` launcher or run the commands below from the extracted folder. On Linux, replace `dankminer.exe` with `./dankminer`. Substitute your own payout address for every `YOUR_...` placeholder.

**Quantus / QTC — GPU, Poseidon2/Goldilocks, PPLNS:**

```text
dankminer.exe -a quantus -w YOUR_QUANTUS_ADDRESS -p stratum+tcp://1miner.net:19835 --worker rig1
```

For Quantus SOLO, use port **19837**. The Windows `mine_quantus.bat` launcher ships with this SOLO endpoint; edit its pool setting if you want PPLNS. Use the TCP endpoints with DankMiner, not the pool's QUIC/UDP ports. [Quantus PPLNS connection page](https://1miner.net/pool/qtc-pplns1/connect) · [Quantus SOLO connection page](https://1miner.net/pool/qtc-solo1/connect)

Fast CUDA is the default. Add `--quantus-exact` for exact CUDA arithmetic; OpenCL uses exact arithmetic. Fast mode can miss rare valid shares, and every submitted GPU candidate is independently verified on the CPU. Set Quantus pool password/options with `--quantus-password TEXT`.

**CapStash — GPU:**

```text
dankminer.exe -a capstash -w YOUR_CAP_ADDRESS -p stratum+tcp://1miner.net:3691 --worker rig1
```

For CapStash SOLO through 1Miner, use `stratum+tcp://1miner.net:3791`.

**Xelis — NVIDIA CUDA GPU:**

```text
dankminer.exe -a xelis -w YOUR_XEL_ADDRESS -p stratum+tcp://1miner.net:4073
```

**Warthog — GPU + CPU, JanusHash:**

```text
dankminer.exe -a warthog -w YOUR_WART_ADDRESS -p stratum+tcp://1miner.net:4200 --worker rig1
```

Warthog uses CUDA or OpenCL for GPU work and CPU workers for VerusHash. Automatic discovery checks OpenCL when CUDA is unavailable. AMD rigs need the appropriate OpenCL driver.

**Monero / XMR — CPU, RandomX rx/0:**

```text
dankminer.exe -a xmr -w YOUR_XMR_ADDRESS -p stratum+tcp://1miner.net:3332 --worker rig1
```

Port `3332` is 1Miner's CPU endpoint; `3333` is its higher-difficulty endpoint. Compatible default-parameter RandomX rx/0 pools can also be used.

**Coin — CPU, WorkRoot / RandomX v2, local node:**

```text
dankminer.exe -a coin -w YOUR_COIN_PAYOUT_ADDRESS --coin-rpc http://127.0.0.1:10005 --cpu-threads 8
```

Coin needs a synced **Patoshi 1.0.5** node with RPC enabled. Match `--coin-rpc` to your node's configured address and port. Cookie discovery is automatic; use `--coin-rpc-cookie PATH` to select a cookie explicitly. The miner does not need private keys or a wallet unlock.

Coin defaults to eight workers and supports **1–15 CPU workers**. Keep `coin_randomx.dll` beside `dankminer.exe` on Windows, or `libcoin_randomx.so` beside `dankminer` on Linux. Coin has **0% developer fee**.

### Configuration files

Edit a JSON sample under `examples/`, then run:

```text
dankminer.exe -c examples/quantus.example.config.txt
```

Explicit CLI options override configuration values. CPU companion options can be added to a GPU-primary configuration command.

---

## Dual Mining

Add **one** CPU companion to a GPU primary: `--xmr-wallet` for Monero or `--coin-wallet` for Coin. GPU primaries are CapStash, Xelis, Warthog and Quantus.

**CapStash GPU + Coin CPU:**

```text
dankminer.exe -a capstash -w YOUR_CAP_ADDRESS -p stratum+tcp://1miner.net:3691 --coin-wallet YOUR_COIN_PAYOUT_ADDRESS --coin-rpc http://127.0.0.1:10005 --coin-threads 8
```

**Quantus GPU + Monero CPU:**

```text
dankminer.exe -a quantus -w YOUR_QUANTUS_ADDRESS -p stratum+tcp://1miner.net:19835 --xmr-wallet YOUR_XMR_ADDRESS --xmr-pool stratum+tcp://1miner.net:3332 --xmr-threads 8
```

Windows includes eight dual launchers named `mine_dual_<CPU>_<GPU>.bat`. Edit the wallet, pool/RPC and thread settings at the top of the chosen file. Linux uses the same options with `./dankminer`; HiveOS uses its extra configuration arguments field.

Each algorithm retains its own developer fee. The dashboard shows both algorithms separately, and CPU console messages use `[XMR]` or `[COIN]`.

**Thread tuning:** compare measured total hashrate with different worker counts on your CPU. Cache, memory, SMT and the other workload affect the result. Warthog already consumes CPU resources; tune its `--cpu-threads` together with the companion's thread count.

### Coin Wallet Rotation

Create a private file with one Coin payout address per line. Blank lines and `#` comments are ignored; exact duplicates are removed. The list repeats in order, with **30 seconds per wallet** by default.

Standalone Coin:

```text
dankminer.exe -a coin -wl coin-wallets.txt --coin-rpc http://127.0.0.1:10005 --wallet-interval 30 --cpu-threads 8
```

Coin alongside CapStash:

```text
dankminer.exe -a capstash -w YOUR_CAP_ADDRESS -p stratum+tcp://1miner.net:3691 --coin-wallet-list coin-wallets.txt --coin-rpc http://127.0.0.1:10005 --coin-threads 8 --wallet-interval 30
```

Wallet rotation assigns mining time to each payout address. It does not guarantee a block or payout to every address.

---

## Common Options

Run `dankminer.exe --help` or `./dankminer --help` for the full CLI.

```text
  -a ALGO                  capstash, xelis, warthog, quantus, xmr or coin
  -w WALLET                Primary payout address
  -p URL                   Primary pool/RPC URL
  -W NAME / --worker NAME  Worker label where supported
  -c FILE                  JSON configuration file
  --cuda=0,1               Select CUDA device indices
  --ocl=0,1                Select OpenCL device indices
  --no-cuda                Disable CUDA
  --no-ocl                 Disable OpenCL
  --force-opencl           Select OpenCL where the algorithm supports it
  --cpu-threads N          Standalone XMR/Coin or Warthog CPU worker count
  --quantus-exact          Exact Quantus CUDA arithmetic
  --quantus-password TEXT  Quantus pool password/options
  -wl FILE                 Standalone Coin payout-wallet list
  --wallet-interval SEC    Coin payout rotation interval
```

**CPU companion options:**

```text
  --xmr-wallet ADDR         Enable the XMR CPU companion
  --xmr-pool URL            XMR pool URL
  --xmr-worker NAME         XMR worker name
  --xmr-threads N           XMR CPU worker count

  --coin-wallet ADDR        Enable the Coin CPU companion with one payout
  --coin-wallet-list FILE   Enable the Coin companion with a payout list
  --coin-threads N          Coin CPU worker count, 1–15
  --coin-rpc URL            Coin node RPC URL
  --coin-rpc-cookie PATH    Coin RPC authentication cookie
```

Xelis requires CUDA; OpenCL-only or disabled-CUDA settings are rejected. Quantus and Warthog exclude CPU-class OpenCL devices and integrated GPUs such as `gfx1036`.

Optional XMR light mode uses the environment setting `DANKMINER_XMR_LIGHT=1`. It uses a 256 MiB cache plus worker/runtime overhead and trades speed for lower memory use.

---

## Hardware and Requirements

| Algorithm | Mining hardware |
|---|---|
| CapStash | NVIDIA CUDA or compatible OpenCL GPU for Stratum mining |
| Xelis | NVIDIA CUDA GPU |
| Warthog | Dedicated CUDA/OpenCL GPU plus CPU |
| Quantus | Dedicated CUDA/OpenCL GPU |
| Monero | x86-64 CPU, RandomX rx/0 |
| Coin | x86-64 CPU, WorkRoot / RandomX v2; synced local node |

GPU mining requires a vendor driver compatible with the GPU and the selected backend. The CUDA build includes native `sm_120` kernels for RTX 50 hardware; use a driver that supports your actual GPU. Hardware coverage is not a blanket guarantee for every NVIDIA, AMD or Intel model.

Windows Coin modes require the **Microsoft Visual C++ v14 x64 runtime**. The tested Linux baseline is **Ubuntu 20.04 / glibc 2.31**; Coin also requires system libstdc++ with `GLIBCXX_3.4.21` or newer and libgcc. HiveOS integration scripts require Bash and Python 3; the native miner itself does not require Python.

Physical GPU validation used an **RTX 4070 SUPER**, with CUDA and OpenCL tests. Multi-device code paths and AMD inventories were also exercised with simulations. Physical RX 5500 XT, multiple dedicated GPUs, dual-Xeon machines and a physical HiveOS installation remain unverified.

---

## 1Miner Pool Connections

The following TCP ports were checked against [1Miner's public pool configuration](https://1miner.net/api/pools) on **2026-10-04**. Use each pool's connection page for current settings.

| Coin | PPLNS TCP port | SOLO TCP port | Connection guide |
|---|---:|---:|---|
| CapStash GPU | `3691` | `3791` | [PPLNS](https://1miner.net/pool/caps1/connect) / [SOLO](https://1miner.net/pool/caps2/connect) |
| Xelis | `4073` | `4074` | [PPLNS](https://1miner.net/pool/xel1/connect) / [SOLO](https://1miner.net/pool/xel2/connect) |
| Warthog | `4200` | `4201` | [PPLNS](https://1miner.net/pool/wart1/connect) / [SOLO](https://1miner.net/pool/wart2/connect) |
| Monero | `3332` CPU / `3333` higher difficulty | — | [PPLNS](https://1miner.net/pool/xmr1/connect) |
| Quantus | `19835` | `19837` | [PPLNS](https://1miner.net/pool/qtc-pplns1/connect) / [SOLO](https://1miner.net/pool/qtc-solo1/connect) |

Use the URL format `stratum+tcp://HOST:PORT`. Quantus ports `19834` and `19836` are QUIC/UDP endpoints; use the TCP ports above with DankMiner. Coin mining in this release uses your node's RPC endpoint.

The [1Miner connection interface](https://1miner.net/pool/qtc-solo1/connect) offers these region hostnames; check the selected pool and region before connecting:

| Region | Hostname |
|---|---|
| North America | `1miner.net` |
| Europe | `eu1.1miner.net` |
| Singapore | `sgp.1miner.net` |

---

## HiveOS

Install DankMiner as a **custom miner** using the HiveOS tarball. Example flight sheet for Quantus PPLNS:

| Field | Value |
|---|---|
| **Miner name** | `dankminer` |
| **Installation URL** | `https://github.com/DankMiner/DankMiner/releases/download/DankMinerV1.5.5/dankminer-1.5.5.tar.gz` |
| **Hash algorithm** | `quantus` |
| **Wallet and worker template** | `%WAL%` or `%WAL%.%WORKER_NAME%` |
| **Pool URL** | `stratum+tcp://1miner.net:19835` |
| **Pass** | `x` |
| **Extra config arguments** | Optional device, arithmetic or CPU companion settings |

The miner name must match **`dankminer`**. The archive contains `h-manifest.conf`, `h-config.sh`, `h-run.sh`, `h-stats.sh` and `h-stop.sh`; keep them beside the executable. No `mine_*.sh` files are needed.

For Monero alongside the selected GPU algorithm, add:

```text
--xmr-wallet YOUR_XMR_ADDRESS --xmr-pool stratum+tcp://1miner.net:3332 --xmr-threads 4
```

For Coin instead, add:

```text
--coin-wallet YOUR_COIN_PAYOUT_ADDRESS --coin-rpc http://127.0.0.1:10005 --coin-threads 4
```

The Coin example assumes a synced node on that rig with discoverable RPC authentication. Adjust the endpoint and cookie path for your setup. Use one CPU companion at a time.

When upgrading, update the installation URL and confirm the startup banner reports **v1.5.5**. Replace old CRB settings with a supported algorithm. Back up local settings before reinstalling a custom miner.

---

## Developer Fees

| Algorithm | Fee |
|---|---:|
| CapStash | 2% |
| Xelis | 1% |
| Warthog | 1% |
| Monero / XMR | 2% |
| Coin | 0% |
| Quantus | 1% |

Each algorithm keeps its own fee schedule when dual mining. Pool fees are separate. Quantus uses one fee timer for the GPU rig, with no additional startup fee.

---

## Troubleshooting

**Warthog stops with CUDA error 801 on an AMD rig:** make sure you are running v1.5.5. This release can use OpenCL when CUDA is unavailable. Install the appropriate AMD OpenCL driver and run an offline check:

```text
dankminer.exe -a warthog --warthog-self-test --force-opencl
```

**Xelis rejects OpenCL or finds no CUDA devices:** Xelis is NVIDIA CUDA-only. Use a compatible NVIDIA GPU and driver, and remove OpenCL-only settings.

**Quantus or Warthog skips `gfx1036` or a CPU OpenCL device:** integrated graphics and CPU-class OpenCL devices are intentionally excluded. Use a dedicated GPU.

**No kernel image / unsupported GPU or driver:** check the selected GPU and update to a vendor driver compatible with that hardware. A single driver version does not establish compatibility across all GPU generations.

**Missing Coin DLL or shared library:** extract the complete package and keep its runtime library beside the miner. On Windows, install the Visual C++ v14 x64 runtime. On Linux, check the libstdc++ and libgcc requirements above.

**GLIBC version error:** both Linux and HiveOS packages use the same native executable and compatibility baseline. Use a compatible OS/runtime; switching between the two archives does not change that requirement.

**Coin cannot connect or reports an unsupported node:** start and sync Patoshi 1.0.5 with RPC enabled. Verify the RPC port and authentication cookie. An RPC connection error is separate from Coin's GPU-independent CPU hashing.

**Coin hashes but has no accepted work:** Coin's counter measures found blocks. A short run may produce none; hashing activity and a complete wallet rotation do not guarantee a block.

**Low CPU hashrate:** compare a few thread counts while watching total dual-mining performance. Coin requests large pages and physical-core placement by default, with ordinary-page fallback. More workers do not always produce more hashes.

**GPU resets or becomes unavailable:** check the driver, temperatures, clocks, power and hardware connections. Resolve the underlying issue before restarting. No watchdog-changing batch file is included in this release.

---

## What Changed in v1.5.5

- Added native Quantus GPU mining, fast/exact CUDA modes, CPU verification and a 1% developer fee.
- Added Warthog OpenCL mining and recovery from unavailable CUDA initialization, plus processor-group-aware CPU placement.
- Corrected Xelis CUDA device selection and early validation of incompatible backend options.
- Excluded CPU-class OpenCL devices and integrated graphics from Quantus and Warthog GPU selection.
- Retained Monero and Coin CPU mining, Coin payout rotation, and eight GPU/CPU combinations.
- Removed CRB / Cereblix mining modes, flags, launchers and NeuroMorph dependency.
- Cleaned public packages: 14 Windows launchers, direct Linux CLI/configuration, and HiveOS integration scripts. Source archives and private settings remain outside the downloads.

See the [v1.5.5 release notes](https://github.com/DankMiner/DankMiner/releases/tag/DankMinerV1.5.5) for the recorded Quantus performance comparison, live dual-mining results and hardware coverage limits.

---

## SHA-256 Checksums

| File | SHA-256 |
|---|---|
| `DankMiner-v1.5.5-Windows.zip` | `82a7baa2c15932959e3688fcc05f8c7967e240d525882276ca60552fefc5aca2` |
| `DankMiner-v1.5.5-Linux.tar.gz` | `b8ab54d0735a4a357924dc89eda027c588ab2cf1b16556bd72d462b6160ddbe9` |
| `dankminer-1.5.5.tar.gz` | `6b2f6257ea6b4d377825097d6498c3f87c7769070558c5c15e65b7acc799c415` |

---

## Links

- **Pools:** [1miner.net](https://1miner.net/pools)
- **Software:** [1Miner software downloads](https://1miner.net/software)
- **Releases and issues:** [DankMiner on GitHub](https://github.com/DankMiner/DankMiner)
- **Quantus:** [Quantus Network](https://github.com/Quantus-Network)
- **CapStash Core:** [CapStash Core on GitHub](https://github.com/CapStash/CapStash-Core)
- **Community:** [1Miner Telegram](https://t.me/OneMinerNet)

© 2026 DankMiner / 1Miner.net
