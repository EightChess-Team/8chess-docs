---
icon: lucide/flask-conical
---

# 8chesstest

**8chesstest** is the EightChess testing framework. It is a fork of [official-stockfish/fishtest](https://github.com/official-stockfish/fishtest), the distributed testing framework used to validate engine changes across thousands of games on donated hardware.

- **Repository:** [EightChess-Team/8chesstest](https://github.com/EightChess-Team/8chesstest)
- **Upstream documentation:** [Fishtest wiki](https://official-stockfish.github.io/docs/fishtest-wiki/Home.html)
- **Language:** Python

## What it does

A engine patch can easily change strength by a few Elo — far too little to judge by eye. 8chesstest runs many games between a **base** engine (current code) and a **test** engine (patched code), then applies a statistical test to decide whether the change is an improvement.

Key ideas:

- **SPRT** (Sequential Probability Ratio Test) decides when enough games have been played.
- **Workers** donated by the community run the games and upload results.
- **PGN files** record every game for later inspection.

## Workflow

1. Build your patched engine and record its `bench` output — see [Regression tests](../8chess/regression-tests.md).
2. [Create a test](creating-a-test.md) with the base and test binaries, a book, time control, and SPRT bounds.
3. Community [workers](running-the-worker.md) pick up the test and play games.
4. Follow the result on the test page; [PGN files](pgn-files.md) are available for analysis.
5. When the test passes or fails, act on the result in your pull request.

## Guides

- [Creating my first test](creating-a-test.md)
- [Running the worker](running-the-worker.md)
- [PGN files](pgn-files.md)
- [FAQ](faq.md)

## Contributing compute

Running a worker is one of the most useful ways to help the project. See [Running the worker](running-the-worker.md) for platform options and the upstream [Fishtest wiki](https://official-stockfish.github.io/docs/fishtest-wiki/Home.html) for detailed setup instructions.

!!! note "Fork specifics"
    8chesstest tracks fishtest. Where this documentation does not cover a detail, the upstream [Fishtest wiki](https://official-stockfish.github.io/docs/fishtest-wiki/Home.html) is the authoritative reference for the shared workflow and tooling.
