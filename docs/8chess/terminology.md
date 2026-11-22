---
icon: lucide/book-open
---

# Terminology

A short glossary of terms used across the 8Chess documentation.

## Engine and evaluation

**8Chess** — A free, open-source UCI chess engine derived from Reckless, with a 1024-wide NNUE evaluation embedded in a single Rust binary.

**UCI (Universal Chess Interface)** — The text protocol used by chess engines and GUIs to communicate. Commands are sent on standard input; responses on standard output.

**GUI** — Graphical user interface. A chess program that displays the board and talks to the engine via UCI. 8Chess does not include one.

**NNUE (Efficiently Updatable Neural Network)** — A neural network architecture whose evaluation can be incrementally updated as moves are made. 8Chess uses a 1024-wide network with piece and threat features.

**Centipawn (cp)** — A unit of evaluation: 100 cp equals roughly one pawn. `score cp 34` means the side to move is evaluated as 0.34 pawns better.

**EvalFile** — The UCI option used to load an external NNUE network; `<internal>` selects the network embedded at build time.

**Feature transformer** — The first NNUE layer, which converts sparse piece/threat features into a 1024-dimensional representation.

**Input / output bucket** — Subsets of network weights selected by king square (input) or piece count (output). 8Chess uses 10 input and 8 output buckets.

## Search

**Alpha-beta search** — The tree-search algorithm engines use to find good moves while pruning branches that cannot affect the result.

**PV (Principal Variation)** — The sequence of moves the engine considers best; reported by `info ... pv ...`.

**MultiPV** — An option that reports several top moves instead of only the best one.

**Nodes / NPS** — Positions searched and nodes per second, respectively — measures of search speed.

**Depth** — How many plies ahead the engine searches.

**Hash / Transposition table** — A cache of previously analyzed positions. The `Hash` option controls its size in MiB.

**Threads** — Parallel search workers. The `Threads` option controls how many are used.

**MoveOverhead** — Extra time reserved when playing with a clock, to account for GUI and OS delays.

## Positions and games

**FEN (Forsyth–Edwards Notation)** — A text format describing a chess position: piece placement, side to move, castling rights, en passant target, halfmove clock, and fullmove number.

**PGN (Portable Game Notation)** — A text format for recording complete games, commonly used for match results.

**UCI move** — A move written as from-square, to-square, and optional promotion piece, e.g. `e2e4`, `g1f3`, `a7a8q`.

**Chess960 / FRC (Fischer Random Chess)** — A chess variant with randomized starting positions. Enabled with `UCI_Chess960`.

**Syzygy tablebases** — Complete endgame databases that can prove perfect play in positions with few pieces. Enabled with `SyzygyPath` in `syzygy` builds.

## Testing and development

**Bench** — A fixed-position benchmark whose node count is used as a fingerprint for engine changes.

**Perft** — A move-generation test that counts leaf nodes at a given depth; used to verify correctness.

**SPRT (Sequential Probability Ratio Test)** — A statistical method used by [8chesstest](../8chesstest/index.md) to decide whether a change improves strength.

**SPSA (Simultaneous Perturbation Stochastic Approximation)** — A tuning method; `spsa` builds expose extra tunable parameters.

**Elo** — A rating scale for comparing engine strength. A difference of ~18.8 ± 16.2 Elo, for example, means the result is not statistically conclusive.

**Binpack** — A compact training-data format (sfbinpack) used by the NNUE training pipeline.
