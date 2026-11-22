---
icon: lucide/database
---

# Useful data

A consolidated list of networks, datasets, suites, and commands used in 8Chess development.

## Networks

| Resource | Link |
| --- | --- |
| Trained NNUE networks, SHA-256 manifest | [huggingface.co/EightChess-Team/eightchess-networks](https://huggingface.co/EightChess-Team/eightchess-networks) |
| Current embedded network | `eightchess_1024_master.nnue` (fine-tune on 968,660 SF/Lc0-labelled positions) |
| Network format specification | [Network format](../nnue/network-format.md) |

Networks are raw, fixed-layout binaries. Load one at runtime with:

```text
setoption name EvalFile value /path/to/network.nnue
```

or embed one at build time with `EVALFILE=/path/to/network.nnue cargo build --release`.

## Training data

| Resource | Link |
| --- | --- |
| Binpack datasets (sfbinpack format) | [huggingface.co/datasets/EightChess-Team/eightchess-data](https://huggingface.co/datasets/EightChess-Team/eightchess-data) |
| Training code | [`training/eightchess_nnue/` in the engine repo](https://github.com/EightChess-Team/8chess/tree/v2/training) |
| Data tools | [`tools/data/` in the engine repo](https://github.com/EightChess-Team/8chess/tree/v2/tools/data) |

Locally, these directories are intentionally not committed: `data/` (FEN sets, SF/Lc0 labels, `.npz` datasets), `checkpoints/`, `networks/`, and `results/`.

## Opening suites and FEN sets

See the [Opening books](../books/index.md) section and the [books repository](https://github.com/EightChess-Team/books):

- `book/30k_openings.fen` — broad 30,000-position suite
- `book/anti_stockfish.fen` — positions avoiding well-trodden Stockfish lines
- `book/uho_sharp.fen` — sharp UHO positions
- `suites/8moves_v3.fen` / `.pgn` — standard 8-ply suite
- `suites/UHO_XXL.pgn.zip` — UHO XXL 2022 suite

The 60k-game goal is played **without** an opening book; these suites are for development, training-data filtering, and match testing.

## Frequently used commands

```sh
# Build (embeds the default network)
cargo build --release

# Build with a specific network
EVALFILE=/path/to/network.nnue cargo build --release

# Verify the embedded network matches the Python reference
PYTHONPATH=training python3 -m eightchess_nnue.verify_engine \
  --engine target/release/eightchess \
  --network networks/eightchess_1024_master.nnue --count 100

# Label FENs with Stockfish
python3 tools/data/eval_fens_sf.py --fens data/fens.txt --out data/sf.tsv \
  --sf /path/to/stockfish --depth 25 --workers 11

# Run local matches
python3 tools/matches/match_runner.py --help
```

## Related sites

- [Project website](https://eightchess-team.github.io/8chess/)
- [Engine source](https://github.com/EightChess-Team/8chess)
- [Testing framework](https://github.com/EightChess-Team/8chesstest)
- [Books repository](https://github.com/EightChess-Team/books)
