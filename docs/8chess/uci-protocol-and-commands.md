---
icon: lucide/terminal
---

# UCI protocol and 8Chess commands

8Chess speaks the [Universal Chess Interface (UCI)](https://backscattering.de/chess/uci/) over standard input/output. This page documents the standard commands it supports, its options, and the extra non-UCI commands useful for development.

## Standard UCI commands

| Command | Description |
| --- | --- |
| `uci` | Identify the engine, list options, and reply with `uciok`. |
| `isready` | Synchronize with the engine; reply is `readyok`. |
| `ucinewgame` | Clear the transposition table, histories, and thread state for a new game. |
| `position ...` | Set the current position (see below). |
| `go ...` | Start a search with the given limits (see below). |
| `stop` | Stop the current search and return the best move so far. |
| `ponderhit` | The opponent played the pondered move; stop searching in ponder mode. |
| `setoption name <name> value <value>` | Change an engine option. Button options take no value. |
| `quit` | Stop the search and exit. |

### `position`

```text
position startpos [moves <uci-move> ...]
position fen <fen> [moves <uci-move> ...]
```

- `startpos` loads the standard starting position.
- `fen` takes the six FEN fields (placement, side to move, castling, en passant, halfmove clock, fullmove number).
- `moves` applies a list of UCI moves (for example `e2e4`, `e7e5`, `g1f3`, `a7a8q`). Invalid moves are ignored.
- With `UCI_Chess960` enabled, castling is also accepted in the king-to-rook encoding used by some GUIs.

### `go`

```text
go depth <n>
go movetime <ms>
go nodes <n>
go mate <n>
go wtime <ms> btime <ms> [winc <ms> binc <ms>] [movestogo <n>]
go infinite | ponder
go searchmoves <move> ... [limits]
```

| Limit | Meaning |
| --- | --- |
| `depth <n>` | Search to exactly this depth. |
| `movetime <ms>` | Search for at most this many milliseconds. |
| `nodes <n>` | Stop after this many nodes. |
| `mate <n>` | Search for a mate in `<n>` moves. |
| `wtime` / `btime` | Clock time in milliseconds for the side to move. |
| `winc` / `binc` | Clock increment in milliseconds. |
| `movestogo` | Moves remaining until the next time control. |
| `infinite` / `ponder` | Search without a limit until `stop`. |
| `searchmoves` | Restrict the search to the listed root moves. |

If no usable clock or limit is supplied (or `infinite` / `ponder` is present), the search runs indefinitely until `stop`.

## Options

Options are reported by `uci` and changed with `setoption`.

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Hash` | spin | 64 (1–262144) | Transposition table size in MiB. |
| `Threads` | spin | 1 (1–available) | Number of search threads. |
| `MoveOverhead` | spin | 10 ms (0–2000) | Time reserved for GUI and OS overhead. |
| `Minimal` | check | false | Reduce `info` output to a minimal format. |
| `Clear Hash` | button | — | Clear the transposition table. |
| `UCI_Chess960` | check | false | Enable Chess960/FRC rules and castling encodings. |
| `MultiPV` | spin | 1 (1–MAX_MOVES) | Report the top N lines instead of just the best move. |
| `EvalFile` | string | `<internal>` | Load an external NNUE file; empty or `<internal>` restores the embedded network. |
| `Contempt` | spin | 0 (−100–100) | Bias the evaluation toward or away from draws. |
| `Sharpness` | spin | 0 (0–100) | Adjust how sharply the engine plays. |
| `SyzygyPath` | string | empty | Path to Syzygy tablebases (only in `syzygy` builds). |
| SPSA parameters | spin | varies | Exposed only in `spsa` builds for tuning. |

Value parsing is case-insensitive for `setoption`, and names and values may contain spaces (for example paths passed to `EvalFile` or `SyzygyPath`).

```text
setoption name Threads value 8
setoption name Hash value 1024
setoption name EvalFile value /path/to/my nets/network.nnue
```

## Non-UCI commands

These commands are available from the console and are mainly useful for debugging and testing.

| Command | Description |
| --- | --- |
| `compiler` | Print compiler version, target, and features. |
| `d` | Print the current board. |
| `eval` | Print NNUE-derived piece values, output buckets, and the final evaluation. |
| `evalnum` | Print the raw NNUE evaluation score for the current position. |
| `dumpacc <white\|black>` | Dump the PST and threat accumulators for a side. |
| `threatprobe <piece> <from> <attacked> <to>` | Inspect a threat feature's base, offset, and attack index. |
| `bench [args]` | Run the standard benchmark (node counts and speed). |
| `speedtest [args]` | Run a raw speed test. |
| `perft <depth>` | Run perft on the current position. |
| `simpleperft <depth>` | Run a simpler perft implementation for cross-checking. |
| `islegalperft <depth>` | Run a legality-checking perft for cross-checking. |

## Example session

```text
> uci
id name 8Chess 2.0.0
id author 8Chess project
option name Hash type spin default 64 min 1 max 262144
option name Threads type spin default 1 min 1 max 16
option name MoveOverhead type spin default 10 min 0 max 2000
option name Minimal type check default false
option name Clear Hash type button
option name UCI_Chess960 type check default false
option name MultiPV type spin default 1 min 1 max 218
option name EvalFile type string default <internal>
option name Contempt type spin default 0 min -100 max 100
option name Sharpness type spin default 0 min 0 max 100
uciok
> position startpos moves e2e4 e7e5 g1f3
> go depth 12
info depth 12 score cp 34 nodes 284512 nps 882000 pv b8c6 f1b5 a7a6
bestmove b8c6
> evalnum
34
> quit
```

Values such as the version, thread maximum, node counts, and evaluations above are illustrative — your output will differ by build and platform.

## See also

- [Download and usage](download-and-usage.md)
- [Advanced topics](advanced-topics.md)
- [Regression tests](regression-tests.md)
- [Terminology](terminology.md)
