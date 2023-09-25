# Rust Approval Starter

## Overview

You are working on some legacy code. Other languages/IDEs provide automatic functionality upon saving that will create updated snapshots and evaluate line coverage.

This starter package shows how we can accomplish this in Rust / VS Code.

The GildedRoses refactoring kata is used as a starting point because the provided code is ideal for showcasing the workflow proposed in this readme.

## Setup

Install command-line cargo programs:

```
cargo install cargo-tarpaulin cargo-make
```

Install Coverage Gutters:

- https://marketplace.visualstudio.com/items?itemName=ryanluker.vscode-coverage-gutters

## Usage

In a terminal window, run coverage:

```
cargo make coverage
```

In another terminal window, run insta:

```
cargo make insta
```

Both of these commands will watch for changes and execute automatically.

Open `gildedrose.rs` and uncomment lines in the test. Upon saving, the insta tab will prompt whether you want to accept the snapshot. If you accept, coverage (tarpaulin) will re-run, updating the coverage information in VS Code.

## Learnings

Rust coverage tools require tests be executed with the flag: `RUSTFLAGS="-Cinstrument-coverage"` (Tarpaulin will set this for you).

When first trying to configure this flow, the challenge was that both insta and tarpaulin (or grcov, for that matter) would rebuild all dependencies when running, resulting in a slow feedback loop.

The solution is to ensure the coverage artifacts are stored in a separate directory. Looking in `Makefile.toml`, we see the output for coverage is set to `"./target/coverage"`. If using other tools, this can be configured by setting the `CARGO_TARGET_DIR` environment variable.

The other caveat for Tarpaulin is that it would clean artifacts before running, so we disable this with `--skip-clean`.

## References

- https://github.com/xd009642/tarpaulin
- https://github.com/sagiegurari/cargo-make
- https://github.com/mitsuhiko/insta
- https://github.com/emilybache/GildedRose-Refactoring-Kata/tree/main/rust/src
