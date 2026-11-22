---
icon: lucide/plus
---

# Creating my first test

This walkthrough describes the typical flow for submitting a playing-strength test to 8chesstest. The web interface follows Stockfish's fishtest, so the upstream [Creating my first test](https://official-stockfish.github.io/docs/fishtest-wiki/Creating-my-first-test.html) guide is also a good reference.

## Before you start

1. Build both engines:
   - **Base:** the current default branch (`v2`) of [8chess](https://github.com/EightChess-Team/8chess).
   - **Test:** your branch with the patch applied.
2. Run `bench` on both and keep the output. The two binaries should use the same network unless the network itself is what you are testing.
3. Make sure your patch is available as a Git diff/commit so it can be referenced in the test.

```sh
cargo build --release
./target/release/eightchess "bench"
```

See [Regression tests](../8chess/regression-tests.md) for the built-in checks.

## Create the test

On the 8chesstest web instance:

1. Open the **Create new test** page.
2. Select the base and test engine commits (or upload binaries, depending on instance configuration).
3. Enter the branch/repository containing your patch.
4. Choose the **book** and time control. 8Chess strength tests are normally run without an opening book; development suites live in the [books](../books/index.md) repository.
5. Set the **SPRT bounds**. Bounds describe the Elo range you care about; standard values are documented in the [Fishtest mathematics](https://official-stockfish.github.io/docs/fishtest-wiki/Fishtest-Mathematics.html) page.
6. Set the number of threads and concurrency options that workers should use.
7. Submit the test.

## Follow the results

The test page shows:

- games played and remaining,
- current score and Elo estimate with a confidence interval,
- the SPRT result (pass, fail, or continue),
- worker activity and any errors.

Do not stop a test early just because the first results look good — SPRT decides when the result is statistically meaningful.

## After the test

- **Pass:** mention the test ID and result in your [pull request](https://github.com/EightChess-Team/8chess/pulls).
- **Fail:** investigate the regression; use [PGN files](pgn-files.md) to inspect lost games.
- **Inconclusive:** consider a longer test with different bounds.

## Checklist

- [ ] Base and test built from the expected commits
- [ ] `bench` output recorded
- [ ] Same engine options on both sides
- [ ] Appropriate book/time control chosen
- [ ] SPRT bounds set
- [ ] Test URL included in the pull request

## See also

- [Running the worker](running-the-worker.md)
- [PGN files](pgn-files.md)
- [8chesstest FAQ](faq.md)
