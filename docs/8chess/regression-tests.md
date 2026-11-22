---
icon: lucide/flask-conical
---

# Regression tests

8Chess ships with several built-in checks and an external distributed testing framework. Use them before and after making changes to the engine or network.

## Built-in commands

Run these from the engine console:

```text
bench
speedtest
perft 5
simpleperft 5
islegalperft 5
```

| Command | Purpose |
| --- | --- |
| `bench` | Standard benchmark: node counts and search speed on a fixed set of positions. |
| `speedtest` | Raw search speed measurement. |
| `perft <depth>` | Move-generation node counts from the current position. |
| `simpleperft <depth>` | Independent perft implementation for cross-checking. |
| `islegalperft <depth>` | Perft with legality checks, useful for debugging move generation. |

`bench` is also the standard fingerprint posted with pull requests to the engine repository — compare it against the base commit to detect unintended search changes.

## Unit tests

```sh
cargo test
```

The test suite covers UCI parsing, FEN handling, move generation (castling, en passant, promotions), and Chess960 castling encodings.

## Network verification

The training tree contains a verifier that compares the Python replica of the network against the compiled engine's `evalnum` output:

```sh
PYTHONPATH=training python3 -m eightchess_nnue.verify_engine \
  --engine target/release/eightchess \
  --network networks/eightchess_1024_master.nnue \
  --count 100
```

This checks exact integer agreement on arbitrary FEN batches. The exporter also performs a float → quantized → binary → round-trip check. See [Training pipeline](../nnue/training.md) and [Network format](../nnue/network-format.md).

## Match testing

The engine repository includes a local match runner:

```sh
python3 tools/matches/match_runner.py --help
```

The standard opening suites and FEN sets live in the [books](https://github.com/EightChess-Team/books) repository and under `tools/` in the engine repository.

### Published match record

| Match (no opening book, `1.0+0.01`) | Result |
| --- | --- |
| 100 games vs Stockfish 19 | 51.0% |
| 500 games vs Stockfish 19 | 47.3% (+57 =359 −84, −18.8 ± 16.2 Elo) |

The project goal is a 60,000-game no-book match against Stockfish 19 with zero Stockfish wins.

## Distributed testing with 8chesstest

For larger tests, the team uses [8chesstest](https://github.com/EightChess-Team/8chesstest), a fork of Stockfish's fishtest framework. It runs SPRT-based tests on donated hardware across many machines. See the [8chesstest documentation](../8chesstest/index.md).

!!! note
    Published results are a snapshot. Check the [engine repository](https://github.com/EightChess-Team/8chess) for current numbers.
