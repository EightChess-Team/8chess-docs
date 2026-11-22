---
icon: lucide/code
---

# Developers

This page is an overview of the 8Chess source tree and the contribution workflow. The canonical source is the [engine repository](https://github.com/EightChess-Team/8chess).

## Repository layout

| Path | Contents |
| --- | --- |
| `src/` | Engine source: board, search, NNUE, threads, UCI, transposition table, time management |
| `src/nnue/` | NNUE inference, accumulators, and threat-feature indexing |
| `src/tools/` | Built-in tools behind `bench`, `speedtest`, and `perft` commands |
| `src/types/` | Core types (moves, squares, pieces, scores) |
| `build/` | Build script that downloads and embeds the network |
| `training/eightchess_nnue/` | PyTorch training code, export, format checks, and engine verification |
| `tools/data/` | Data preparation helpers (labeling, filtering, dataset construction) |
| `tools/matches/` | Local match runner and related scripts |
| `deps/Fathom` | Syzygy tablebase probing dependency |
| `docs/` | Project website and NNUE format specification |
| `kaggle/` | Kaggle-related training helpers |

## Building and testing

```sh
cargo build --release
cargo test
./target/release/eightchess "bench"
```

See [Compiling from source](compiling-from-source.md) and [Regression tests](regression-tests.md) for details and optional features.

## Contributing

1. Open or find an [issue](https://github.com/EightChess-Team/8chess/issues) describing the change.
2. Fork the repository and create a topic branch from the default branch (`v2`).
3. Make your change, keeping the code formatted with `cargo fmt`.
4. Run `cargo test` and `bench`; include the bench output in your pull request.
5. For playing-strength changes, run a test with [8chesstest](../8chesstest/index.md) where possible.
6. Open a [pull request](https://github.com/EightChess-Team/8chess/pulls) with a clear description and results.

!!! note "Benchmark fingerprints"
    `bench` node counts are the standard way to prove a patch changes only what it claims to. Always compare against the base commit for search-related changes.

## Using 8Chess in your own project

There are three common approaches:

- **UCI subprocess:** launch the `eightchess` binary and exchange UCI commands over stdin/stdout. See [UCI protocol and 8Chess commands](uci-protocol-and-commands.md).
- **Library code:** the crate exposes a library (`src/lib.rs`) with reusable board and search components.
- **WebAssembly:** `src/wasm.rs` provides a wasm target for browser or embedded scenarios.

If you distribute a modified build or link the engine into your own program, observe the AGPL-3.0 terms described below.

## Training and data entry points

```sh
python3 tools/data/eval_fens_sf.py --fens data/fens.txt --out data/sf.tsv \
  --sf /path/to/stockfish --depth 25 --workers 11
python3 tools/data/build_hq25.py --help
PYTHONPATH=training python3 -m eightchess_nnue.train --help
PYTHONPATH=training python3 -m eightchess_nnue.verify_engine \
  --engine target/release/eightchess \
  --network networks/eightchess_1024_master.nnue --count 100
python3 tools/matches/match_runner.py --help
```

See [Training pipeline](../nnue/training.md) for the full workflow.

## Development resources

- [NNUE network format](../nnue/network-format.md)
- [8chesstest framework](../8chesstest/index.md)
- [Opening books](../books/index.md)
- [Useful data](useful-data.md)

## License

8Chess is licensed under the **GNU Affero General Public License v3.0 (AGPL-3.0)**, inherited from Reckless. You may read, modify, and redistribute the code under that license; see the [LICENSE file](https://github.com/EightChess-Team/8chess/blob/v2/LICENSE) and the upstream project for the original engine and network tooling.
