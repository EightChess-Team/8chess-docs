---
icon: lucide/server
---

# Running the worker

Workers are the machines that play the games for 8chesstest. Donating compute is one of the most valuable ways to contribute to 8Chess development.

!!! note
    8chesstest is a fishtest fork. For step-by-step, platform-specific setup, use the upstream guides linked below — they apply directly to the shared fishtest worker software.

## Choose a setup

| Platform | Guide |
| --- | --- |
| Docker | [Run the worker using Docker](https://official-stockfish.github.io/docs/fishtest-wiki/Run-the-worker-using-Docker.html) |
| Linux | [Running the worker on Linux](https://official-stockfish.github.io/docs/fishtest-wiki/Running-the-worker-on-Linux.html) |
| macOS | [Running the worker on macOS](https://official-stockfish.github.io/docs/fishtest-wiki/Running-the-worker-on-macOS.html) |
| Windows | [Running the worker on Windows](https://official-stockfish.github.io/docs/fishtest-wiki/Running-the-worker-on-Windows.html) |
| Amazon AWS EC2 | [Running the worker in the AWS EC2 cloud](https://official-stockfish.github.io/docs/fishtest-wiki/Running-the-worker-in-the-Amazon-AWS-EC2-cloud.html) |
| Google Colaboratory | [Run the worker on Google Colaboratory](https://official-stockfish.github.io/docs/fishtest-wiki/Run-the-worker-on-Google-Colaboratory.html) |

## General steps

1. Create an account on the 8chesstest instance.
2. Install the worker following the guide for your platform.
3. Configure the worker with your credentials, the number of cores to use, and a unique worker name.
4. Start the worker and confirm it appears as **connected** on the workers page.
5. Leave it running; it will pick up available tests automatically.

## Resource guidelines

- Give the worker the cores you can spare — more cores mean more games, but heavy load can slow other work.
- Make sure the machine has enough memory for the engine's hash allocation.
- Keep the worker updated; the framework occasionally changes its protocol.
- If a machine overheats or becomes unstable, lower the core count or stop the worker.

## Troubleshooting

- **Worker does not connect:** check the server URL, credentials, and network/firewall settings.
- **Games fail immediately:** confirm the engine binaries download correctly and that the platform matches the worker OS/architecture.
- **Very low NPS:** check CPU throttling, other load, and whether too many threads were assigned.

## See also

- [Creating my first test](creating-a-test.md)
- [PGN files](pgn-files.md)
- [8chesstest FAQ](faq.md)
- [Contributing to fishtest](https://official-stockfish.github.io/docs/fishtest-wiki/Contributing-to-Fishtest.html)
