---
icon: lucide/home
---

# 8Chess Docs

Documentation, technical details, and frequent questions for **8Chess** — a free and open-source, 1024-wide NNUE chess engine.

8Chess is an experimental engine derived from [Reckless](https://github.com/codedeliveryservice/Reckless). It ships as a single self-contained Rust binary: the neural network is embedded at build time, so there are no runtime model files to download or configure.

!!! tip "New to 8Chess?"
    Start with [Download and usage](8chess/download-and-usage.md) to play in any UCI-compatible GUI, then read the [UCI protocol and 8Chess commands](8chess/uci-protocol-and-commands.md) reference.

## Guides

**For players**

- [Download and usage](8chess/download-and-usage.md) — get the engine and install it in a GUI
- [UCI protocol and 8Chess commands](8chess/uci-protocol-and-commands.md) — every command and option
- [8Chess FAQ](8chess/faq.md) — common questions
- [Terminology](8chess/terminology.md) — glossary of engine and testing terms

**For developers and contributors**

- [Compiling from source](8chess/compiling-from-source.md) — build the engine yourself
- [Advanced topics](8chess/advanced-topics.md) — NNUE, search, tablebases, Chess960
- [Developers](8chess/developers.md) — code layout and contribution workflow
- [Regression tests](8chess/regression-tests.md) — bench, perft, matches, verification

**For researchers and testers**

- [NNUE training](nnue/index.md) — training pipeline and [network format](nnue/network-format.md)
- [8chesstest](8chesstest/index.md) — the distributed testing framework
- [Opening books](books/index.md) — opening suites and FEN sets
- [Useful data](8chess/useful-data.md) — networks, datasets, and links

## Support

If you're looking for assistance with 8Chess:

- [GitHub Issues](https://github.com/EightChess-Team/8chess/issues) — bug reports and feature requests
- [GitHub Pull requests](https://github.com/EightChess-Team/8chess/pulls) — patches and discussion
- [Project website](https://eightchess-team.github.io/8chess/) — downloads, news, and online play

## Related repositories

| Repository | Description |
| --- | --- |
| [8chess](https://github.com/EightChess-Team/8chess) | The engine source code, training tools, and project site |
| [8chesstest](https://github.com/EightChess-Team/8chesstest) | The EightChess testing framework (fishtest fork) |
| [books](https://github.com/EightChess-Team/books) | Opening books and FEN suites |
| [nnue-pytorch](https://github.com/EightChess-Team/8chess/tree/v2/training) | Training code lives under `training/` in the engine repo |

## License

8Chess is licensed under **AGPL-3.0**, inherited from Reckless. You can read the code, build it yourself, and run your own matches.
