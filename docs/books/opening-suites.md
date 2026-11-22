---
icon: lucide/list
---

# Opening suites

All suites live in the [books repository](https://github.com/EightChess-Team/books), in `book/` and `suites/`.

## `book/`

| File | Description |
| --- | --- |
| `30k_openings.fen` | Broad 30,000-position opening suite. |
| `anti_stockfish.fen` | Curated positions intended to avoid well-trodden Stockfish lines. |
| `uho_sharp.fen` | Sharp/forcing UHO positions. |

## `suites/`

| File | Description |
| --- | --- |
| `8moves_v3.fen` and `8moves_v3.pgn` | Standard 8-ply opening suite. |
| `UHO_XXL.pgn.zip` | UHO XXL 2022 (+110/+139) suite, zipped. |

## Formats

- **FEN** files contain **one position per line** — six FEN fields each. They can be fed directly to match tools.
- **PGN** files contain complete games; the move sequence can be replayed or converted to FEN positions.

A FEN line can be loaded into 8Chess with:

```text
position fen <six FEN fields>
go depth 20
```

See [UCI protocol and 8Chess commands](../8chess/uci-protocol-and-commands.md).

## Choosing a suite

| Goal | Suggested suite |
| --- | --- |
| Broad coverage across many openings | `30k_openings.fen` |
| Explore positions outside Stockfish's main lines | `anti_stockfish.fen` |
| Test tactical / forcing play | `uho_sharp.fen` or `UHO_XXL.pgn.zip` |
| Standard short opening phase | `8moves_v3.fen` / `.pgn` |

## Notes

- These are test and training inputs, not a "book" loaded by the engine at runtime.
- The 60k-game goal is played without a book.
- Sources include Stockfish/fishtest books and UHO collections; original upstream licenses apply.

## See also

- [Opening books](index.md)
- [Useful data](../8chess/useful-data.md)
- [Regression tests](../8chess/regression-tests.md)
