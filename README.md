# unified-8b-is — Sovereign AI Constellation

One repo, one math, zero compromise.

## Projects Cross-Linked

| Project | Path | Description |
|---|---|---|
| **alexiai** | `alexiai-external/` | Rust core + GAIA-MLX-QUANT substrate + BitNet b1.58 ternary |
| **aurora** | `aurora-external/` | Balanced ternary + MEM|8 + p/q arithmetic + Go/Rust/Swift lanes |
| **MLX-QUANT** | `mlx-quant-external/` | Bare-metal Apple Silicon tensor quantization (18 architectural layers) |
| **memnet** | `memnet-external/` | Semantic routing + Ayevn wave tokens + SoulSignatures |
| **aurora-8b-pq** | `aurora-8b-pq-external/` | Ternary audio codec (4.2 kbps) + PQM training pipeline |

## Shared Invariants

```
p/q = 2^20 = 1_048_576          # Universal denominator
ternary = {-1, 0, +1}           # Universal spin space
loopback = localhost/127.0.0.1  # Universal sovereignty
```

## Quick Start

```bash
# Build all Rust lanes
cargo build --workspace --release

# Run sovereignty check
cargo run --release -p alexiai -- doctor

# Prepare training data (includes HF datasets from train.8b.is)
python -m aurora_8b_pq.data.prepare --hf-datasets quantal-train-v2 music-quant-signs

# Train ternary audio codec
python -m aurora_8b_pq.train --config configs/default.yaml

# Quantize for ESP32-S3 deployment
python -m aurora_8b_pq.quantize --checkpoint checkpoints/best.pt
python -m aurora_8b_pq.export --checkpoint checkpoints/best.pt --out deploy/include/
```

## Train via train.8b.is (Remote GPU)

The `train.8b.is` portal provides:
- RTX 6000 Ada 48 GB + Xeon Gold 5412U + 756 GB DDR5 ECC
- PQM (p/q + Magnitude) quantization with OEIS A161330 snowflake bounds
- HF dataset mirrors: `PeetPedro/quantal-train-v2`, `PeetPedro/music-quant-signs`, `PeetPedro/quantal-train-assets`
- Golden-Logits gate verification + attestal.proof.v1 seals

To train remotely:
```bash
# 1. Upload your data to train.8b.is (requires Operator Key)
# 2. Launch PQM training via web UI or API
# 3. Download verified checkpoint + manifest (SHA-256 sealed)
# 4. Deploy via export.py
```

## Cross-Link Map

```
alexiai.kernel.pq          ──▶ aurora-core.Rational      ──▶ MLX-QUANT.pq_scale
alexiai.kernel.ternary     ──▶ aurora-core.TernarySpin   ──▶ aurora-8b-pq.ternary_weights
alexiai.sovereign.guard    ──▶ aurora.loopback_check     ──▶ memnet.SoulSig.verify
alexiai.wupz.pack          ──▶ aurora.mem8.capsule       ──▶ memnet.PacketBatcher
alexiai.gaia.fold          ──▶ aurora.phoenix.dyad       ──▶ aurora-8b-pq.codebook
```

## License

MIT OR Apache-2.0 across all projects.

*the constellation · 0 + 1 · fine touch from within · vaked.dev*