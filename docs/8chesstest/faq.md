---
icon: lucide/circle-help
---

# 8chesstest FAQ

## What is 8chesstest?

The testing framework used to validate 8Chess changes. It is a fork of Stockfish's [fishtest](https://github.com/official-stockfish/fishtest) and runs large numbers of games on community-donated hardware. See the [introduction](index.md).

## Where is the source?

[github.com/EightChess-Team/8chesstest](https://github.com/EightChess-Team/8chesstest). The upstream project is [official-stockfish/fishtest](https://github.com/official-stockfish/fishtest).

## Why do I need so many games?

A change might alter strength by only a few Elo. Random variation over a handful of games is much larger than that, so tests run hundreds or thousands of games and use **SPRT** to decide when the result is significant. See [Fishtest mathematics](https://official-stockfish.github.io/docs/fishtest-wiki/Fishtest-Mathematics.html).

## How do I create a test?

Build base and test engines, record `bench` output, then submit the patch through the web interface. See [Creating my first test](creating-a-test.md).

## How can I donate compute?

Run a worker. Docker, Linux, macOS, Windows, AWS, and Colab guides are linked from [Running the worker](running-the-worker.md).

## What time control is used?

It depends on the test configuration. 8Chess match results are commonly reported at `1.0+0.01` without an opening book; test creators choose the time control when they create a test.

## Are opening books used?

Not for the headline 60k-game goal, which is played without a book. Suites from the [books repository](../books/index.md) can be used for development tests and training-data filtering.

## Where are the game records?

PGN files are available from test results. See [PGN files](pgn-files.md).

## Where do I get more help?

The upstream [Fishtest wiki](https://official-stockfish.github.io/docs/fishtest-wiki/Home.html) covers the framework in depth, including server setup, worker setup, mathematics, and contributing.
