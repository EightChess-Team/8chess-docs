---
icon: lucide/graduation-cap
---

# Training pipeline

The training code lives in `training/eightchess_nnue/` in the [engine repository](https://github.com/EightChess-Team/8chess). It encodes positions into the sparse piece/threat feature format, trains soft-target WDL heads, and exports the binary layout consumed by `src/nnue.rs`.

## Data

Data and checkpoints are intentionally not committed to the repository:

| Directory | Contents |
| --- | --- |
| `data/` | FEN sets, SF/Lc0 labels, encoded `.npz` datasets |
| `checkpoints/` | Training checkpoints (`.nnue`, `.pt`) |
| `networks/` | Local network copies, including Lc0 networks |
| `results/` | PGN match records |

Public datasets are available from [huggingface.co/datasets/EightChess-Team/eightchess-data](https://huggingface.co/datasets/EightChess-Team/eightchess-data) in sfbinpack format. Opening suites for filtering and match testing live in the [books](../books/index.md) repository.

### Labeling FENs

```sh
python3 tools/data/eval_fens_sf.py --fens data/fens.txt --out data/sf.tsv \
  --sf /path/to/stockfish --depth 25 --workers 11
python3 tools/data/build_hq25.py --help
```

## Training

```sh
PYTHONPATH=training python3 -m eightchess_nnue.train --help
```

The trainer uses PyTorch. The export must respect the quantization conventions in the [network format](network-format.md#4-quantized-training-conventions):

- piece and threat weights use the same `255 = 1.0` scale,
- threat and L1 weights are 8-bit and clamped to `[-127, 127]`,
- later layers are stored as float32.

Keep piece weights within ±128 (i16) and threat weights within ±0.498 (i8) so no information is lost at export time.

## Verification

The Python replica is compared against the compiled engine's `evalnum` output:

```sh
PYTHONPATH=training python3 -m eightchess_nnue.verify_engine \
  --engine target/release/eightchess \
  --network networks/eightchess_1024_master.nnue --count 100
```

The current format reports a **100% exact integer match** on arbitrary FEN batches. `training/eightchess_nnue/format.py` also asserts every binary offset and the total file size.

## Export

`training/eightchess_nnue/export.py` performs an end-to-end round trip:

```text
floats → quantized ints → binary file → parse back → quantized forward
```

It reports the quantization error so you can judge whether a new network is faithfully represented. The result is a raw 84,349,504-byte file with the layout described in [Network format](network-format.md#5-binary-layout).

## Publishing

Trained networks are published to [huggingface.co/EightChess-Team/eightchess-networks](https://huggingface.co/EightChess-Team/eightchess-networks) with a SHA-256 manifest. At build time, a network can be embedded with `EVALFILE`, or selected at runtime with the `EvalFile` UCI option.

## Testing strength

After producing a network:

1. Verify engine agreement (`verify_engine`).
2. Run built-in checks (`bench`, `perft`) — see [Regression tests](../8chess/regression-tests.md).
3. Run matches with `tools/matches/match_runner.py`.
4. For significant changes, submit a test to [8chesstest](../8chesstest/index.md).

```sh
python3 tools/matches/match_runner.py --help
```

## See also

- [Network format](network-format.md)
- [Useful data](../8chess/useful-data.md)
- [Developers](../8chess/developers.md)
