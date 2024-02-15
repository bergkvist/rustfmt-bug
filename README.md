# Demonstration of bug in rustfmt

*as of rust version 1.78.0-nightly (ee9c7c940 2024-02-14)*

## Problem
When running `cargo fmt` on this project you will get a warning message in your output:
```
`fn_call_width` cannot have a value that exceeds `max_width`. `fn_call_width` will be set to the same value as `max_width`
```
This message is wrong, as `fn_call_width=90` is less than `max_width=100`. See `.rustfmt.toml`.

## How to reproduce
```
cargo fmt
```
or
```
rustfmt src/main.rs
```

By modifying `max_width` in `.rustfmt.toml` to 101, you will still see the error message. Changing it to 102 makes the error message go away.

Another way of making the error go away is by modifying the macro to:
```rs
mod a {
    macro_rules! b {
        () => {{}};
    }
}
```
essentially adding another pair of brackets to the match rule.
