---
icon: lucide/book-open
---

# Opening books

The [books repository](https://github.com/EightChess-Team/books) contains opening suites used for 8Chess development, training-data filtering, and match testing.

!!! note "Not used for the headline match"
    The 8Chess 60,000-game goal is played **without** an opening book. These suites are development and testing inputs.

| Resource | Link |
| --- | --- |
| Repository | [EightChess-Team/books](https://github.com/EightChess-Team/books) |
| Opening suites | [Opening suites](opening-suites.md) |
| Engine repository | [EightChess-Team/8chess](https://github.com/EightChess-Team/8chess) |

## Uses

- **Development matches** — start games from a spread of diverse openings.
- **Training-data filtering** — select positions of interest, such as sharp or anti-Stockfish lines.
- **Regression testing** — replay the same opening set across engine versions.

## License note

Sources include Stockfish/fishtest books and UHO collections. Original upstream licenses apply to those suites; check the repository for details before redistributing.

## See also

- [Opening suites](opening-suites.md)
- [Useful data](../8chess/useful-data.md)
- [8chesstest](../8chesstest/index.md)
