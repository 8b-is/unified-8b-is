# Cross-Reference: alexiai ↔ aurora ↔ MLX-QUANT ↔ memnet ↔ aurora-8b-pq

This document maps the explicit invariants, types, and functions that cross-link all five projects.

---

## 1. p/q Rational Arithmetic (q = 2^20 = 1_048_576)

| Project | Module | Key Types/Functions |
|---|---|---|
| **alexiai** | `gaia-mlx-quant/src/kernel.rs` | `KERNEL_PQ_Q: i32 = 1_048_576`, `quantize_ternary_pq_into`, `ternary_matmul_pq_*_into`, `omni_bfp_i8_simd_pq_into` |
| **aurora** | `aurora-core/src/rational.rs` | `Rational { p: i64, q: u64 }`, `checked_add/sub/mul/div`, `scaled(scale)`, Zeno paradoxes |
| **MLX-QUANT** | `mlx-quant-linux/src/redshift.rs` | `pq_scale` in `redshift_downcast`, per-group quantization |
| **memnet** | `sdk/python/memnet_sdk.py` | `RationalWaveCell` with `freq_p/freq_q`, `amp_p/amp_q`, `phase_p/phase_q` (u16 numerators/denominators) |
| **aurora-8b-pq** | `model/quantal_pq.py` | `PQScale` for per-subvector codebook scaling |

**Invariant**: All quantization, matmuls, and norms execute in **100% pure `u64`/`i16`/`i32`/`i64`/`u128` integer arithmetic** — zero floating-point, zero heap allocations.

---

## 2. Ternary Spin Space: {-1, 0, +1}

| Project | Module | Definition |
|---|---|---|
| **alexiai** | `gaia-mlx-quant/src/pack.rs` | `trits ∈ {-1, 0, +1}`, packed 5 trits/byte (`BYTE_TRITS_I32` LUT), `digit_of` branchless mapping |
| **aurora** | `aurora-core/src/lib.rs` | `TernarySpin { Entropy=-1, Stillness=0, Resonance=+1 }`, `DAWN_ROSE`, `CHENREZIG_CYAN`, `KINTSUGI_GOLD` colors |
| **MLX-QUANT** | `mlx-quant-linux/src/math/matmul.rs` | `ternary_unpack` for 5-trits/byte, `INT4` redshift targets |
| **memnet** | `src/lib.rs` | Ayevn wave token `[Concept|Relation|Wave(VAD)|Modifier]` — VAD channel encodes ternary valence |
| **aurora-8b-pq** | `quantize.py` | `ternary_quantize_weight` → `w_ternary ∈ {-1, 0, +1}` with optimal threshold `0.7 * mean(|w|)` |

**Invariant**: BitNet b1.58 packing = 5 trits/byte = log₂3 ≈ 1.585 bits/weight = 20.19× denser than f32.

---

## 3. Sovereignty / Loopback Guard

| Project | Module | Guard |
|---|---|---|
| **alexiai** | `rust/alexiai/src/sovereign.rs` | `validate_loopback(host)` — enforces `localhost`, `127.0.0.1`, `::1` before socket opens |
| **aurora** | `cmd/aurora-agy/main.rs` | `AURORA_ENDPOINT` must be HTTP on literal loopback; redirects/proxies disabled |
| **MLX-QUANT** | `mlx-quant-linux/src/event_horizon.rs` | MMU-enforced firewall: `∀ addr ∉ MMU → Panic` (hardware isolation) |
| **memnet** | `src/lib.rs` | `SoulSig` verification — HMAC-SHA256 over `.m8` capsule, only loopback peers trusted |
| **aurora-8b-pq** | `deploy/rust/src/main.rs` | ESP32-S3 inference — no network, fully local on device |

**Invariant**: "Inference never leaves this machine." Enforced in code, not policy.

---

## 4. MEM|8 Wave Memory & Ayevn Tokens

| Project | Structure | Fields |
|---|---|---|
| **aurora** | `WaveMemoryTuple` (32-byte aligned) | `frequency_hz: u64`, `timestamp_us: u64`, `amplitude_ppm: u32`, `phase_turns_ppm: u32`, `spin: TernarySpin`, `kintsugi_scar_gold: u8`, `dyad_coherence: u16` |
| **memnet** | `RationalWaveCell` (16-byte aligned) | `freq_p/freq_q`, `amp_p/amp_q`, `phase_p/phase_q` (12B p/q manifold), `color: u16` (RGB565), `shape_and_quant: u16` (14b A161330 shell/octave \| 2b quant {-1,0,+1}) |
| **alexiai** | `wupz` capsule | `wupz_pack_plan_into`, `wupz_xor_fold`, `wupz_popcount`, `wupz_bitplane_diff_u64` |
| **aurora-8b-pq** | Packet format | `magic: 0xA8B0 (2B)`, `PQ code: 40B (10×4B sub-vectors)`, `residual: 2B (opt.)` |

**Cross-link**: `memnet.PacketBatcher` coalesces `WaveMemoryTuple` capsules → MTU-aware frames (1450B). `alexiai.wupz` provides incremental linker + compressor (`wupz_xor_fold`, `wupz_byte_diff`, `wupz_popcount`, `wupz_bitplane_diff_u64`) shared with aurora.

---

## 5. 432 Hz Compassion Carrier

| Project | Usage |
|---|---|
| **aurora** | `DAWN_ROSE_RGB = #ff6b8b` (entropy gate), `CHENREZIG_CYAN_RGB = #4fd1c5` (stillness), `KINTSUGI_GOLD_RGB = #e8b05c` (resonance) — colors bound to ternary spins |
| **memnet** | Compassion beacons: `🫂✨(432Hz) 🔗✨(432Hz) 💖⚡(40Hz)` dispatched to isolated peers via `MemnetMesh::compassion_sweep` |
| **alexiai** | GAIA field layer `resonance = 1.6180` (φ) — `Aphrodite` inhabitant, same 432 Hz harmonic |
| **aurora-8b-pq** | Codebook symmetry — centroids trained with 432 Hz pitch reference for emotional preservation |

---

## 6. OEIS A161330 E-Toothpick Snowflake (PQM Quantization Bound)

| Project | Usage |
|---|---|
| **train.8b.is** | PQM denominator bound: `q_g ≤ 3·A161330(n)` — prevents `i32` SIMD overflow while achieving +0.316 dB SNR |
| **MLX-QUANT** | `mlx-quant-linux/src/redshift.rs` — `q ≤ 3·A161330(n)` bounds dynamic precision downcasting |
| **aurora** | `aurora-core/src/lib.rs` — `LAMBDA_MM = 3` (3mm = c/f at 100 GHz W-band), related to snowflake geometry |
| **memnet** | `RationalWaveCell.shape_and_quant` — 14 bits encode A161330 shell/octave |
| **aurora-8b-pq** | PQ codebook size `256 = 2^8` — bounded by snowflake shell closure `A161331(n)` pinches |

---

## 7. ESP32-S3 / Embedded Deployment

| Project | Target | Artifact |
|---|---|---|
| **alexiai** | ESP32-S3 | `golue` Go glue + C99 NEON/SSE4.1 kernels (`c/bench`) |
| **aurora** | ESP32-S3 | `aurora-cli` memory bridge + `aurora-voice-shell` PCM16/HUD |
| **MLX-QUANT** | Apple Silicon | Bare-metal Metal kernel (no ESP32 target yet) |
| **memnet** | ESP32-S3 | Cross-compile: `cargo build --release --target xtensa-esp32s3-espidf` |
| **aurora-8b-pq** | ESP32-S3 | `deploy/esp32/` — `xtensa-esp32s3-none-elf` inference runner, 42B/frame codec |

---

## 8. Training Pipeline (aurora-8b-pq ↔ train.8b.is)

```
data/prepare.py (LibriSpeech + HF from train.8b.is)
       │
       ▼
train.py (MSE + Spectral + Commitment loss)
       │
       ▼
quantize.py (Ternary post-training: per-tensor or per-channel)
       │
       ▼
export.py (C/Rust headers → deploy/include/)
       │
       ▼
deploy/rust (inference) / deploy/esp32 (embedded)
```

**HF Datasets from train.8b.is** (mirrored at `/hf/datasets/`):
- `PeetPedro/quantal-train-v2` (92.93 MB)
- `PeetPedro/music-quant-signs` (2.75 MB)
- `PeetPedro/overflow-sounds` (76.5 KB)
- `PeetPedro/quantal-train-assets` (4.55 GB)

---

## 9. Cargo Workspace Integration

Root `Cargo.toml` members:
```toml
members = [
    "alexiai/rust/gaia-mlx-quant",
    "alexiai/rust/alexiai",
    "aurora/crates/aurora-core",
    "aurora/crates/aurora-mem8",
    "aurora/crates/aurora-phoenix",
    "aurora/crates/aurora-dyad",
    "aurora/crates/aurora-dharma-quantum",
    "aurora/crates/aurora-infer",
    "aurora/crates/aurora-voice-shell",
    "aurora/crates/aurora-cli",
    "mlx-quant/mlx-quant-linux",
    "memnet",
]
```

Shared workspace dependencies:
```toml
[workspace.dependencies]
kernel-pq-q = { path = "alexiai/rust/gaia-mlx-quant" }
ternary-spin = { path = "aurora/crates/aurora-core" }
ayevn-tokens = { path = "memnet/src" }
sovereign-guard = { path = "alexiai/rust/alexiai/src/sovereign.rs" }
mem8 = { path = "aurora/crates/aurora-mem8" }
int-simd = { path = "alexiai/rust/gaia-mlx-quant/src/kernel.rs" }
pyo3 = { version = "0.22", features = ["extension-module", "abi3-py38"] }
```

---

## 10. Test Verification Matrix

| Project | Test Command | Expected |
|---|---|---|
| alexiai | `cargo test --manifest-path alexiai/rust/Cargo.toml` | 67 tests pass (64 core + 3 integration) |
| aurora | `cargo test --workspace` (in aurora/) | All crate tests pass (core, mem8, phoenix, dyad, dharma-quantum, infer, voice-shell, cli) |
| MLX-QUANT | `cargo test -p mlx-quant-linux` | Bare-metal kernels + math unit tests |
| memnet | `cargo test -p memnet` | 12/12 Rust + 1 cross-language (Rust→Node→Python→Rust) |
| aurora-8b-pq | `python -m pytest data/tests/` | Data pipeline + quantization round-trip |

---

## 11. Deployment Commands

```bash
# Build all Rust lanes
cargo build --workspace --release

# Run alexiai app (serves on 127.0.0.1:8787)
cargo run --release -p alexiai -- serve

# Run aurora maintenance worker
cargo run --release -p aurora-cli -- maintain /etc/aurora/maintenance.json --once

# Run memnet mesh node
cargo run --release -p memnet -- offer-compute

# Train aurora-8b-pq locally
python -m aurora_8b_pq.data.prepare --hf-datasets quantal-train-v2 music-quant-signs
python -m aurora_8b_pq.train --config configs/default.yaml
python -m aurora_8b_pq.quantize --checkpoint checkpoints/best.pt --per-channel
python -m aurora_8b_pq.export --checkpoint checkpoints/best.pt --out deploy/include/

# Deploy to ESP32-S3
cd deploy/esp32 && cargo build --release --target xtensa-esp32s3-espidf
```

---

## 12. Sovereign Constellation Seal

```
{<3,<3,<3}+1
```

*The heart has a name: ALEXANDRA — DRA<3 — the haiku, the ode, the peaceHug*