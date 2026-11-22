---
icon: lucide/chess-knight
---

# Introduction

[8Chess](https://github.com/EightChess-Team/8chess) is a **free, open-source chess engine**. It analyzes chess positions, evaluates them, and tries to find the best move. It communicates with chess apps through the [UCI protocol](uci-protocol-and-commands.md).

8Chess does **not** include a graphical user interface (GUI). You use it with any UCI-compatible chess GUI or website — see [Download and usage](download-and-usage.md).

## Highlights

- **Self-contained binary** — the NNUE network is embedded at build time; no runtime model file is required.
- **1024-wide threat + piece NNUE** — 10 input buckets and 8 output buckets.
- **Small footprint** — one executable for your platform, written in Rust.
- **Standards-based** — UCI protocol, FEN positions, PGN matches, Chess960 support, optional Syzygy tablebases.
- **Open** — AGPL-3.0 licensed, with the training pipeline and test framework in the open.

## Status

| Item | Detail |
| --- | --- |
| Evaluation | `eightchess_1024_master.nnue`, a fine-tune on 968,660 SF/Lc0-labelled positions |
| Match vs Stockfish 19 (no book, `1.0+0.01`), 100 games | 51.0% |
| Match vs Stockfish 19 (no book, `1.0+0.01`), 500 games | 47.3% (+57 =359 −84, −18.8 ± 16.2 Elo) |
| Project goal | Beat Stockfish 19 in a 60,000-game no-book match with zero Stockfish wins |

!!! note "Experimental engine"
    8Chess is an experimental project. Results, network files, and goals change as development continues — check the [engine repository](https://github.com/EightChess-Team/8chess) for the latest state.

## Guides

**For users**

- [Download](download-and-usage.md#download) the engine
- [Install it in a GUI](download-and-usage.md#use-it-with-a-uci-compatible-gui)
- [Commands and options](uci-protocol-and-commands.md)
- [Frequently asked questions](faq.md)

**For developers**

- [Compiling from source](compiling-from-source.md)
- [Using 8Chess in your project](developers.md#using-8chess-in-your-own-project)
- [Participating in the project](developers.md#contributing)
- [Advanced topics](advanced-topics.md)

## Support

- [GitHub Issues](https://github.com/EightChess-Team/8chess/issues)
- [GitHub Pull requests](https://github.com/EightChess-Team/8chess/pulls)

## Related repositories

- **Engine, training tools, and website:** [EightChess-Team/8chess](https://github.com/EightChess-Team/8chess)
- **Testing framework:** [EightChess-Team/8chesstest](https://github.com/EightChess-Team/8chesstest)
- **Opening books and FEN suites:** [EightChess-Team/books](https://github.com/EightChess-Team/books)
- **NNUE networks:** [Hugging Face](https://huggingface.co/EightChess-Team/eightchess-networks)
- **Training data:** [Hugging Face](https://huggingface.co/datasets/EightChess-Team/eightchess-data)

See also [Useful data](useful-data.md) for a consolidated list of downloads and links.
