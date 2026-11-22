---
icon: lucide/cpu
---

# Advanced topics

This page collects deeper technical details about how 8Chess works: evaluation, search, threading, tablebases, and Chess960.

## Evaluation: 1024-wide threat + piece NNUE

The evaluation is a neural network trained on labelled positions and embedded in the binary. It combines **piece features** and **threat features** in a 1024-wide feature transformer.

```text
piece features (10 x 768)   ──┐
                              ├──> feature transformer (1024) ──> clipped-pair
threat features (66864)     ──┘                                   activation (1024)
                                                                       │
                                               input-clipped affine (1024 -> 16)  x 8 output buckets
                                                                      │
                                                         affine (16 -> 32) x 8 buckets
                                                                      │
                                                          affine (32 -> 1) x 8 buckets
                                                                      │
                                                       score = output * 380
```

Key properties:

- **Feature transformer output:** 1024 neurons, split into two halves of 512.
- **10 input buckets** selected by the POV king square; **8 output buckets** selected by the total piece count.
- **Threat features are not input-bucketed** — they share weights across king zones.
- **Activation:** clipped-pair squaring in the transformer, clipped ReLU in later layers.

The current embedded network, `eightchess_1024_master.nnue`, is a fine-tune on 968,660 Stockfish/Lc0-labelled positions.

For the complete architecture, quantization scheme, and binary layout, see [Network format](../nnue/network-format.md).

### Inspecting the evaluation

```text
eval       # NNUE-derived piece values, per-bucket scores, final evaluation
evalnum    # raw evaluation score in centipawns
dumpacc white
threatprobe N e4 e5 f6
```

## Search

8Chess uses an alpha-beta search with aggressive pruning, iterative deepening, a transposition table, and history heuristics. Search state is reset with `ucinewgame` and when an external network is loaded via `EvalFile`.

- **Threads** run parallel searches. The engine votes between threads to pick the final best move when not searching to a fixed depth.
- **MultiPV** reports the top N principal variations.
- **Contempt** and **Sharpness** bias the search/evaluation toward different game styles.
- **MoveOverhead** reserves time for communication overhead in clocked games.

## Tablebases (Syzygy)

Build with `--features syzygy` to enable the `SyzygyPath` option:

```text
setoption name SyzygyPath value /path/to/syzygy
```

The engine reports the number of pieces loaded when initialization succeeds. See [Compiling from source](compiling-from-source.md#optional-build-features).

## Chess960 / FRC

Set `UCI_Chess960` to `true` to enable Chess960 rules:

```text
setoption name UCI_Chess960 value true
position fen r3k2r/8/8/8/8/8/8/R3K2R w KQkq - 0 1
```

In Chess960 mode both castling encodings are accepted: king-to-rook (native) and king-to-castling-square (used by some GUIs). Parts of the NNUE feature indexing also mirror the king's file for kingside positions.

## WebAssembly

The source includes a `wasm.rs` module, so the engine can be compiled for WebAssembly targets for browser or embedded use. The standard UCI message loop is excluded on `wasm32` targets.

## Tuning (SPSA)

Build with `--features spsa` to expose additional tunable parameters as UCI options. These are intended for automated tuning and are not needed for normal play.

## See also

- [Network format](../nnue/network-format.md)
- [Training pipeline](../nnue/training.md)
- [UCI protocol and 8Chess commands](uci-protocol-and-commands.md)
- [Terminology](terminology.md)
