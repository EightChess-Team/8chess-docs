---
icon: lucide/hammer
---

# Compiling from source

8Chess is a single Rust binary. Building requires a recent stable Rust toolchain and Git.

## Basic build

```sh
git clone https://github.com/EightChess-Team/8chess.git
cd 8chess
cargo build --release
```

The executable is written to `target/release/eightchess`. Check it with:

```sh
./target/release/eightchess "uci"
```

## The network file

`build/build.rs` downloads the current network from the network release if `networks/eightchess_1024_master.nnue` is missing. The resulting `.nnue` is embedded into the binary at compile time — there is no runtime file dependency.

| Method | Effect |
| --- | --- |
| Let `build.rs` download the network | Default; fetches the current master network |
| Place a compatible `.nnue` in `networks/` | Uses your local file at build time |
| `EVALFILE=/path/to/network.nnue cargo build --release` | Overrides the network used for the build |

To test an external network **without rebuilding**, use the UCI option:

```text
setoption name EvalFile value /path/to/network.nnue
```

Pass an empty value or `<internal>` to go back to the embedded network:

```text
setoption name EvalFile value <internal>
```

!!! warning "Network compatibility"
    Network files are raw, fixed-layout binary images. The engine rejects files whose size differs from the expected 84,349,504 bytes. See [Network format](../nnue/network-format.md) for the full specification.

## Optional build features

| Feature | Purpose |
| --- | --- |
| `syzygy` | Enables the `SyzygyPath` option and Syzygy tablebase probing |
| `spsa` | Exposes tunable search parameters as UCI options for tuning |

```sh
cargo build --release --features syzygy
cargo build --release --features spsa
```

## Verify the build

After building, run the built-in checks:

```text
bench
perft 5
evalnum
```

`bench` prints node counts and speed, `perft` verifies move generation, and `evalnum` prints the raw NNUE evaluation of the current position. See [Regression tests](regression-tests.md) for details.

## Development build

```sh
cargo build          # debug build, faster to compile
cargo test           # unit tests, including UCI parsing and move generation
cargo fmt            # format Rust sources (rustfmt.toml is provided)
```

## Troubleshooting

- **Download fails during build:** place a compatible `.nnue` in `networks/` or set `EVALFILE`.
- **Unknown CPU optimizations:** use a recent stable Rust toolchain and rebuild with `cargo build --release`.
- **Wrong network loaded at runtime:** check `EvalFile`; set it to `<internal>` to use the embedded network.
