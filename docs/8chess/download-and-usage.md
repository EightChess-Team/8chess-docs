---
icon: lucide/download
---

# Download and usage

## Download

8Chess is distributed as a single executable. The network is embedded, so the binary is all you need.

- **Source and releases:** [EightChess-Team/8chess](https://github.com/EightChess-Team/8chess)
- **Release builds:** [GitHub releases](https://github.com/EightChess-Team/8chess/releases)
- **Project site:** [eightchess-team.github.io/8chess](https://eightchess-team.github.io/8chess/)

Prefer to build it yourself? See [Compiling from source](compiling-from-source.md).

### Networks and data

| Resource | Location |
| --- | --- |
| Trained NNUE networks (SHA-256 manifest) | [huggingface.co/EightChess-Team/eightchess-networks](https://huggingface.co/EightChess-Team/eightchess-networks) |
| Training data (binpacks in sfbinpack format) | [huggingface.co/datasets/EightChess-Team/eightchess-data](https://huggingface.co/datasets/EightChess-Team/eightchess-data) |
| Opening books and FEN suites | [EightChess-Team/books](https://github.com/EightChess-Team/books) |

## Use it with a UCI-compatible GUI

8Chess is a command-line engine that speaks [UCI](uci-protocol-and-commands.md). It does not draw a chessboard — your GUI does that. Any UCI-compatible GUI works, including Lichess and Chess.com analysis boards, En Croissant, ChessBase, Arena, Cute Chess, and others.

=== "Typical setup"

    1. Download the engine binary for your platform.
    2. In your GUI, open **Engines** / **Engine management** and choose **Add** (UCI).
    3. Select the `eightchess` executable and confirm.
    4. Set the number of threads and hash size if desired (see [Options](uci-protocol-and-commands.md#options)).
    5. Start a new analysis or a game against the engine.

!!! tip "No network file needed"
    You do **not** need to place a `.nnue` file next to the engine. The default network is compiled into the binary. To experiment with a different network, use the `EvalFile` option instead.

## Run it from the terminal

Start the executable and speak UCI on standard input/output:

```text
$ ./eightchess
> uci
id name 8Chess 2.0.0
id author 8Chess project
option name Hash type spin default 64 min 1 max 262144
option name Threads type spin default 1 min 1 max 16
option name EvalFile type string default <internal>
uciok
> isready
readyok
> position startpos moves e2e4 e7e5 g1f3
> go depth 12
info depth 12 score cp 34 nodes 284512 nps 882000 pv b8c6 f1b5 a7a6
bestmove b8c6
> quit
```

The engine reports its **evaluation** in centipawns and its **best move** in UCI notation. Your GUI turns these into the analysis you see on screen.

It also accepts commands directly as arguments, which is handy for scripting:

```sh
./eightchess "position startpos" "go depth 10"
```

## Play online

The [project website](https://eightchess-team.github.io/8chess/) links to the live browser player, where the engine runs with volunteer-powered compute. See the website for the current play address.

## Next steps

- [UCI protocol and 8Chess commands](uci-protocol-and-commands.md)
- [Advanced topics](advanced-topics.md)
- [FAQ](faq.md)
