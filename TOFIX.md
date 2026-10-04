# TOFIX

Findings from a code scan on 2026-10-04.

## Medium

- `src/main.rs:1-3` - the binary is still `cargo new`'s "Hello, world!", yet the crate has been released four times (tags `v0.1.2`..`v0.1.4`, commit `43fe4b8`) with `release.toml:15` `publish = true`, i.e. every `cargo release` pushes a hello-world crate named `rslily` to crates.io and attaches hello-world binaries to a GitHub release. Stop releasing until there is real functionality (start with parsing a minimal LilyPond subset), or set `publish = false` in `release.toml` for now.
- `src/main.rs:1` - `build.rs` (fleet-shared, `rs-build-rs` check) exports `GIT_SHA`, `GIT_BRANCH`, `GIT_DIRTY`, `GIT_DESCRIBE`, `RUSTC_SEMVER`, `RUST_EDITION`, `BUILD_TIMESTAMP` (`build.rs:89-95`), but `main.rs` reads none of them, so every build forks git/rustc for nothing. Add the fleet's `--version` output that prints these via `env!(...)`, as the other rs* binaries do.

## Low

- `src/main.rs:11-14` - the only test calls `main()` and asserts nothing (it prints "Hello, world!" during the test run). Replace with real tests once there is parsing/engraving code.
- `Cargo.toml:6` - `description = "Rust lilypond version"` (also `config/project.lua:3`, `docs/src/introduction.md:3`, `docs/book.toml:5`) does not say what the tool does; and there are no `keywords`/`categories`/`readme` keys for the crates.io page. Use the README's wording ("A Rust take on LilyPond music engraving") and add `keywords = ["lilypond", "music", "notation"]`, `categories = ["multimedia::audio"]` (or similar).
- `docs/src/introduction.md:1-3` - the mdBook that `README.md:8` points readers to has a one-line introduction and nothing else; state the project status and intended scope there, as README does.
