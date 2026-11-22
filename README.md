# 8Chess Docs

Documentation site for the [8Chess](https://github.com/EightChess-Team/8chess) chess engine, built with [Zensical](https://zensical.org/).

The structure follows the style of the [Stockfish documentation site](https://official-stockfish.github.io/docs/): grouped sidebar sections for the engine, its testing framework, NNUE training, and opening books — all written for 8Chess.

## Sections

- **8Chess** — introduction, download and usage, compiling from source, UCI commands, advanced topics, regression tests, terminology, FAQ, developers, useful data
- **8chesstest** — the fishtest-based testing framework
- **NNUE training** — network format specification and training pipeline
- **Opening books** — opening suites maintained by the team

## Local development

```sh
uv sync
uv run zensical serve
```

Build the static site into `site/`:

```sh
uv run zensical build --clean
```

## Deployment

The [documentation workflow](.github/workflows/docs.yml) builds the site and publishes it to GitHub Pages on pushes to `main` or `master`.

## License

Documentation content is maintained by the 8Chess team. The 8Chess engine itself is licensed under AGPL-3.0.
