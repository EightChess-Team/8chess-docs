---
icon: lucide/file-text
---

# PGN files

Every game played by 8chesstest is recorded as a **PGN** (Portable Game Notation) file. PGNs let you see exactly what happened in a game, reproduce a position, and look for patterns across a match.

## Finding PGNs

PGN files are attached to test results on the 8chesstest instance. From a test page, open the results table and download the PGN archive for the test. The upstream [PGN Files](https://official-stockfish.github.io/docs/fishtest-wiki/PGN-Files.html) wiki page explains the available links and formats in more detail.

## What a PGN contains

A typical engine-vs-engine PGN records:

- event and test identifiers,
- the two engine versions/commits,
- starting position or opening (if a book was used),
- the full move sequence in algebraic notation,
- the result (`1-0`, `0-1`, or `1/2-1/2`),
- comments or headers with extra metadata.

## Inspecting games

You can open PGNs in any chess GUI, or analyze positions with 8Chess over UCI:

```sh
./target/release/eightchess
```

```text
position startpos moves e2e4 e7e5 g1f3 b8c6
go depth 20
```

To reproduce a game from a PGN without retyping moves, export the position as FEN and use:

```text
position fen <six FEN fields>
```

See [UCI protocol and 8Chess commands](../8chess/uci-protocol-and-commands.md) for the full command reference.

## Common uses

- **Understand a regression:** look at games the test engine lost to find a recurring tactical or positional theme.
- **Check opening balance:** verify that both sides get comparable positions when a book is used.
- **Report a bug:** attach the PGN and the relevant FEN to a [GitHub issue](https://github.com/EightChess-Team/8chess/issues).

## See also

- [Creating my first test](creating-a-test.md)
- [Terminology](../8chess/terminology.md)
- [Opening books](../books/index.md)
