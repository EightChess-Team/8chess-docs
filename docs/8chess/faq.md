---
icon: lucide/circle-help
---

# Frequently asked questions

## General

### What is 8Chess?

8Chess is a free, open-source chess engine. It evaluates positions and chooses moves, communicating with chess applications over the [UCI protocol](uci-protocol-and-commands.md). It is written in Rust and derived from [Reckless](https://github.com/codedeliveryservice/Reckless). See the [introduction](index.md).

### Does 8Chess include a graphical interface?

No. Like most engines, it is a command-line program. Use any UCI-compatible GUI — on desktop or web — to see the board and enter moves. See [Download and usage](download-and-usage.md#use-it-with-a-uci-compatible-gui).

### Do I need to download a `.nnue` network file?

No. The network is embedded in the binary at build time. To try a different network, use `setoption name EvalFile value /path/to/network.nnue`, or use the `EVALFILE` environment variable when building from source.

### How strong is 8Chess?

In published no-book matches at `1.0+0.01`, 8Chess scored 51.0% over 100 games and 47.3% over 500 games against Stockfish 19. The project goal is to beat Stockfish 19 in a 60,000-game no-book match with zero Stockfish wins. See [Regression tests](regression-tests.md).

### Which platforms are supported?

8Chess is written in Rust and can be built wherever Rust is supported. Release binaries and build instructions are available in the [engine repository](https://github.com/EightChess-Team/8chess); see [Compiling from source](compiling-from-source.md).

## Usage

### How do I add 8Chess to my GUI?

Download the binary, then use your GUI's "Add engine (UCI)" dialog and select the executable. No extra configuration is required to start; optionally set `Threads` and `Hash`. See [Download and usage](download-and-usage.md).

### Can I use it on Lichess, Chess.com, or En Croissant?

8Chess speaks standard UCI, so it works with interfaces that let you load a local engine. See the [usage guide](download-and-usage.md#use-it-with-a-uci-compatible-gui). The [project site](https://eightchess-team.github.io/8chess/) also links to a browser player.

### Does it support Chess960?

Yes. Set `UCI_Chess960` to `true`:

```text
setoption name UCI_Chess960 value true
```

See [Advanced topics](advanced-topics.md#chess960-frc).

### Can I use Syzygy tablebases?

Yes, in a build made with `cargo build --release --features syzygy`, then set `SyzygyPath`. See [Compiling from source](compiling-from-source.md#optional-build-features).

### What are `Eval`, `evalnum`, and `bench`?

They are non-UCI diagnostic commands: `eval` shows a detailed NNUE breakdown, `evalnum` prints the raw evaluation score, and `bench` runs the standard benchmark. See [UCI protocol and 8Chess commands](uci-protocol-and-commands.md#non-uci-commands).

## Contributing

### How can I help?

- Test the engine and report bugs in [GitHub Issues](https://github.com/EightChess-Team/8chess/issues).
- Submit patches via [pull requests](https://github.com/EightChess-Team/8chess/pulls), including `bench` output.
- Run tests on the distributed framework — see [8chesstest](../8chesstest/index.md).
- Help improve the networks and training data — see [NNUE training](../nnue/index.md).

### Where do I get networks and training data?

From the team's Hugging Face repositories: [networks](https://huggingface.co/EightChess-Team/eightchess-networks) and [data](https://huggingface.co/datasets/EightChess-Team/eightchess-data). Opening suites are in the [books](https://github.com/EightChess-Team/books) repository. See [Useful data](useful-data.md).

### What license is 8Chess under?

**AGPL-3.0**, inherited from Reckless. See the [LICENSE](https://github.com/EightChess-Team/8chess/blob/v2/LICENSE) and the [developers page](developers.md#license).

### How do I report a problem usefully?

Include the engine version (`uci`), your OS and platform, the exact commands or FEN that reproduce the issue, and any relevant `bench` output.
