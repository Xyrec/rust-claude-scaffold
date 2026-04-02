# CLAUDE.md — Rust Project Rules

## Build Commands
- `cargo check` — fast compilation verification (preferred over `cargo build`)
- `cargo clippy --all-targets --all-features -- -D warnings` — linting (zero warnings allowed)
- `cargo test` — run test suite
- `cargo fmt` — format code

## Compilation Feedback
This project uses hooks that automatically run `cargo check` after every file you edit.
- If you see "✓ cargo check passed" — continue working.
- If you see "✗ cargo check failed" — fix the errors shown before editing other files.
- If you see "⚠ cargo check passed with warnings" — fix warnings if they're trivial, note them if not.
- **Do not run `cargo check` manually** unless you need the full verbose output. The hook handles it.

## Code Conventions
- Error handling: use `thiserror` for library error types, `anyhow` for binary/application code
- **Never** use `.unwrap()` in production code. Use `.expect("invariant: reason")` only when the invariant is documented.
- Propagate errors with `?` — do not catch and re-wrap unless adding meaningful context
- Async runtime: `tokio`. Logging: `tracing` crate.
- Prefer `impl Trait` in argument position over generic type parameters when the type is used once

## Architecture Rules
- Keep files under 500 lines. Split into submodules if a file grows beyond this.
- Define types and traits first, implement logic second.
- One public type per module unless types are tightly coupled.
- Feature flags for optional functionality — do not use `cfg` blocks inline, isolate behind module boundaries.

## What NOT to Do
- Do not add dependencies without asking. Check if an existing dependency covers the need.
- Do not fight the borrow checker with `Rc<RefCell<>>` or `Arc<Mutex<>>` as a first resort. Restructure ownership first.
- Do not use `unsafe` unless explicitly approved.
- Do not create deeply nested generic types. If a type signature exceeds one line, introduce a type alias.
