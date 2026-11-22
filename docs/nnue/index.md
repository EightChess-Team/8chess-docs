---
icon: lucide/brain
---

# NNUE training

This section documents the neural network at the heart of 8Chess: how it is trained, how it is exported, and the exact binary format the engine consumes.

The training code lives in `training/eightchess_nnue/` in the [engine repository](https://github.com/EightChess-Team/8chess). It is written in PyTorch and encodes positions into the sparse piece/threat feature format, trains soft-target WDL heads, and exports the binary layout consumed by `src/nnue.rs`.

## Pages

- [Network format](network-format.md) — architecture, features, quantization, and binary layout
- [Training pipeline](training.md) — data preparation, training, verification, and export

## Quick facts

| Item | Value |
| --- | --- |
| Architecture | 1024-wide feature transformer with piece + threat features |
| Input buckets | 10 (by POV king square) |
| Output buckets | 8 (by piece count) |
| Piece features | 7,680 (10 × 768) |
| Threat features | 66,864 |
| Current network | `eightchess_1024_master.nnue`, fine-tuned on 968,660 SF/Lc0-labelled positions |
| Network file size | 84,349,504 bytes, fixed layout |
| Runtime dependency | None — embedded in the binary at build time |

## Resources

- [Networks on Hugging Face](https://huggingface.co/EightChess-Team/eightchess-networks)
- [Training data on Hugging Face](https://huggingface.co/datasets/EightChess-Team/eightchess-data)
- [Training source tree](https://github.com/EightChess-Team/8chess/tree/v2/training)
- [Upstream Reckless project](https://github.com/codedeliveryservice/Reckless)

## Verification

The Python reference implementation is continuously checked against the compiled engine's `evalnum` command; the current format reports a 100% exact-match rate. See [Training pipeline](training.md#verification) and [Regression tests](../8chess/regression-tests.md#network-verification).
