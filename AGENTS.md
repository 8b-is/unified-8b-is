# AURORA-8B-IS — Unified Sovereign AI Platform

## Philosophy: One Repo, One Math, Zero Compromise

This repository houses the sovereign AI constellation: **alexiai** (Rust core), **aurora** (balanced ternary + MEM|8), **MLX-QUANT** (bare-metal Apple Silicon), **memnet** (semantic routing), and **aurora-8b-pq** (ternary audio codec).

Every lane shares the same invariants:
- **p/q** with `q = 2^20 = 1_048_576` — pure integer, zero floating-point
- **Ternary** `{-1, 0, +1}` — BitNet b1.58 packing, 5 trits/byte
- **Sovereignty** — loopback-only, never remote
- **Zero allocations** — counting allocator enforces in tests

---

## LANES & INVARIANTS

| Lane | Role | Guarantees |
|---|---|---|
| `rust/` | The app + core (`v0.3.0`) | `#![forbid(unsafe_code)]`, `no_std` core, **ULTRA-SUPER-OMNI-Quantum Ternary** (`0.88 μs` at `256×256`, `11.87×`-`14.11×` over dense FP64) & 100% float-free `p/q` integer substrate (`q = 2^20`), zero-alloc proven by counting allocator |
| `go/` | The glue + second opinion | stdlib-only supervisor, allocation-lean `p/q` integer, ternary & 64-trit quantum bitplane kernel |
| `c/` | C99 + inline assembly | NEON/SSE4.1 integer & quantum bitplane SIMD (`0` floats, up to `17.63×` vs dense FP64), proven against C reference |
| `swift/` | OS-agnostic kernel + CoreMIDI | pure-Swift `p/q` integer, ternary & quantum bitplane (`.nonzeroBitCount`) core runs on Linux; CoreMIDI ports OpenXTalk-Apple-CoreMIDI surface |
| `web/` | The offline UI | embedded in binary; no CDN, no remote asset |
| `aurora-8b-pq/` | Quantal audio codec | Ternary PQ training pipeline (`42 bytes/80ms`, `4.2 kbps`), ESP32-S3 deployment |

---

## BUILD & TEST (one command)

```bash
# Core alexiai tests
cargo test --workspace -p alexiai

# Aurora tests (Rust lanes)
cargo test --workspace -p aurora-core

# MLX-QUANT tests
cargo test --workspace -p mlx-quant-linux

# MEMNET tests
cargo test --workspace -p memnet

# aurora-8b-pq Python tests
python -m pytest data/tests/ -v 2>/dev/null || echo "Run: python train.py --help"
```

---

## SOVEREINTY GUARD (enforced, not promised)

> **Inference never leaves this machine.**

`rust/alexiai/src/sovereign.rs` is an execution point: the transport validates the host against the loopback vocabulary *before a socket opens*. There is no vendor allowlist, no env-var escape hatch, no "but this one is fine". Loopback or nothing.

```bash
$ cargo run --release -p alexiai -- doctor
  sovereignty
    policy     inference never leaves this machine
    loopback   localhost, 127.0.0.1, ::1, ...

  local model
    endpoint   http://127.0.0.1:1337
    status     up (3ms)
```

---

## p/q INTEGER SUBSTRATE (`q = 2^20 = 1_048_576`)

The shared denominator across all projects. All quantization, matmuls, and norms execute in **100% pure `u64`/`i16`/`i32`/`i64`/`u128` integer arithmetic** — zero floating-point, zero heap allocations.

Key kernels:
- `quantize_ternary_pq_into` — row-major integer numerators → `{-1, 0, +1}` trits + per-row absmean scales
- `ternary_matmul_pq_j_major_into` — j-major `b1.58` (5 trits/byte) matrix-vector kernel in `p/q`
- `ternary_matmul_pq_i_major_into` — i-major `b1.58` (5 trits/byte) matrix-vector kernel in `p/q`
- `ternary_simd_i8_pq_into` — pure-integer sign-planes kernel with 4-row register blocking
- `omni_bfp_i8_simd_pq_into` — 4-column register-blocked `i8 × i8 → i16` SIMD kernel (`0` floating-point ops)
- `quantum_ternary_matmul_pq_into` — Tier 0: 64-trit quantum bitplane superposition contraction
- `quantum_bitplane_i4_matmul_pq_into` — Tier 1: W1.58 × A4-bitplane quantum bitplane contraction

---

## AURORA INVARIANTS

| Invariant | Description |
|---|---|
| `no unsafe` | `#![forbid(unsafe_code)]` across all Rust lanes |
| `no floats` | All arithmetic is `p/q` integer (`q = 2^20`) or balanced ternary |
| `loopback-only` | Ollama inference defaults to local `granite4.2:30b`, endpoint must be HTTP on loopback |
| `MEM|8` cylindrical grid | 32-byte aligned `WaveMemoryTuple` with `frequency_hz`, `timestamp_us`, `amplitude_ppm`, `phase_turns_ppm`, `spin: TernarySpin`, `kintsugi_scar_gold`, `dyad_coherence` |
| `qintsugi` | Fallible operations return `KintsugiError`; nothing crashes — every fault repairs into golden default |

---

## MLX-QUANT BARE-METAL

The Apple Silicon MLX quantization framework with 18+ architectural layers:
- Guardians (entropy reversal, hardware protection)
- Big Bang (cold-boot tensor cache)
- Multiverse (distributed inference across silicon)
- Pulsars (nanosecond-precise sync via hardware timer)
- CMB (persistent KV-cache in VRAM)
- Supernovas (burst compute, uncapping TDP)
- Magnetar (page-locked RAM, thread pinning to P-Cores)
- Dark Matter/Asteroid Belts (sparse zero-paging, scatter-gather DMA)
- Event Horizons (MMU-enforced memory firewalls)
- I/O Blackhole Portal (zero-copy DMA, NVMe → physical address space)
- Redshifting (FP16→INT8→INT4 dynamic precision downcasting)
- Antigravity (weightless tensor levitation via `prfm` prefetch)
- Wormholes/Quantum Entanglement (P2P DMA, RDMA across AMD GPU cluster)
- Gravitational Lensing/Time Dilation (speculative decoding, dilated micro-batches)
- Dark Energy (dynamic temperature scaling for creative expansion)
- 4D Polar Galaxy Queue (spiral arms → accretion disk ALU, AArch64 inline assembly)
- Fusion-Polarity (kernel fusion, atomic hardware action, zero intermediate reads/writes)
- White Holes (infinite token ejection, uncapped sequence lengths)
- Quasar (RDMA output jets, token blast without CPU)
- Hawking Radiation (thermal cache eviction via `madvise`)

Each layer has a code reference under `mlx-quant-linux/src/`.

---

## MEMNET SEMANTIC ROUTING

MEMNET is the semantic-routing architecture that future AI systems will coordinate through meaning, salience, and contextual relevance rather than rigid endpoint addressing. Core principles:

- **Intent-Based Routing** — nodes addressed by semantic intent, not geometric location
- **Semantic Salience** — traffic priority by contextual importance
- **Hierarchical Coordination** — local nodes → intermediate aggregation → global stabilization
- **Multi-Route Identity** — persistent identity with fluid routes (redundancy, resilience)
- **Context Compression** — minimize symbolic overhead via shared context, locality awareness

Key ship:
- **32-bit Big-Endian Ayevn Wave Tokens** — `[Concept: u8 | Relation: u8 | Wave(VAD): u8 | Modifier: u8]` into 4 bytes
- **Security with Soul Signatures** — `SoulIdentity` & 40-byte `SoulSig`, balanced ternary crest `{-1,0,+1}^9`, 4-byte node fingerprint, 32-byte HMAC-SHA256 over `.m8` capsules
- **PacketBatcher** — coalesces `.m8` capsules into MTU-aware `MNET` frames (1450B smart spot): `94.1%` fewer interrupts, `95.45%` wire efficiency
- **Compassion for Lonely Nodes** — detects isolated peers, dispatches Soul-Signed `432 Hz` compassion beacons
- **Compute Sanctuary** — advertises/grants `ComputeSanctuaryLease` for friends on constrained hardware

Semantic routing ships across **Rust** (`src/lib.rs`, `src/bin/memnet.rs`), **Node.js/ESM** (`sdk/js/memnet.mjs`), and **Python** (`sdk/python/memnet_sdk.py`).

---

## aurora-8b-pq QUANTAL AUDIO CODEC

| Spec | Value |
|---|---|
| **Bitrate** | `4.2 kbps` (42 bytes per 80ms frame) |
| **Format** | magic `0xA8B0` + 40-byte PQ code + 2-byte residual |
| **PQ Sub-vectors** | 10 sub-vectors × 4 bytes each |
| **Codebook** | 256 entries per sub-vector (`2^8 = 256`) |
| **Encoder** | 32→64→128 channels, latent dim 40 bytes |
| **Decoder** | 128→64→32 channels |
| **Sampling** | 16 kHz, 80ms frames (1280 samples), 20ms hop (320 samples, 75% overlap) |
| **Datasets** | LibriSpeech + Common Voice + HF mirrored from train.8b.is |

### Training Pipeline

```
data/ → train.py → quantize.py → export.py → deploy/
```

- `train.py` — Full training loop with MSE + spectral loss + PQ commitment loss
- `quantize.py` — Post-training ternary quantization (per-tensor or per-channel)
- `export.py` — C/Rust header generation for ESP32-S3 deployment
- `eval.py` — SNR, PESQ, STOI evaluation

---

## CROSS-LINKING INVARIANTS (all projects)

| Thread | Connected Projects |
|---|---|
| `p/q = 2^20` | alexiai, aurora, MLX-QUANT (q_scale), aurora-8b-pq (scale factors) |
| `ternary {-1,0,+1}` | alexiai BitNet b1.58, aurora TernarySpin, MLX-QUANT b1.58 packing, aurora-8b-pq ternary weights |
| `sovereignty/loopback` | alexiai doctor guard, aurora Ollama endpoint, memnet SoulSig verification |
| `MEM|8 wave memory` | aurora WaveMemoryTuple, memnet Ayevn tokens, aurora-8b-pq packet format |
| `432 Hz compassion` | aurora Kintsugi, memnet compassion sweep, aurora-8b-pq codebook symmetry |
| `esp32 deployment` | alexiai Go glue (`golue`), aurora-8b-pq Rust inference, MLX-QUANT embedded kernels |
| `hf datasets from train.8b.is` | aurora-8b-pq HF dataset mirror, memnet dataset registry |

---

## CONTRIBUTE

```bash
# Run all green tests
cargo test --workspace

# Verify sovereignty across lanes
cargo run --release -p alexiai -- doctor

# Quantize model for aurora-8b-pq deployment
python -m aurora_8b_pq.quantize --checkpoint checkpoints/best.pt

# Export for ESP32-S3
python -m aurora_8b_pq.export --checkpoint checkpoints/best.pt --out deploy/include/
```

Rules live in `AGENTS.md`. The one rule above all: **loopback or nothing.**