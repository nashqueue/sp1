# Common Issues

## Alloy Errors

If you are using a library that depends on `alloy_sol_types`, and encounter an error like this:

```
perhaps two different versions of crate `alloy_sol_types` are being used?
```

This is likely due to two different versions of `alloy_sol_types` being used. To fix this, you can set `default-features` to `false` for the `sp1-sdk` dependency in your `Cargo.toml`.

```toml
[dependencies]
sp1-sdk = { version = "0.1.0", default-features = false }
```

This will configure out the `network` feature which will remove the dependency on `alloy_sol_types` 
and configure out the `NetworkProver`.


## Rust Version Errors

If you are using `alloy` or another library that has an MSRV (minimum support rust version) of 1.76.0
or higher, you may encounter an error like this when building your program.

```
package `alloy v0.1.1 cannot be built because it requires rustc 1.76 or newer, while the currently active rustc version is 1.75.0-nightly`
```

This is due to the fact that the Succinct Rust toolchain is built with version 1.75, which is older
than the MSRV of the `alloy` crate. Note: Once the Succinct Rust toolchain is updated, this error will
go away.

To fix this, you can:

- If using `cargo prove build` directly, pass the `--ignore-rust-version` flag:
  ```
  cargo prove build --ignore-rust-version
  ```
- If using `build_program`, set `ignore_rust_version` to true inside the `BuildArgs` struct and use `build_program_with_args`:
  ```rust
  let args = BuildArgs {
      ignore_rust_version: true,
      ..Default::default()
  };
  build_program_with_args("path/to/program", args);
  ```

## Stack Overflow Errors
If you encounter the following in a script using `sp1-sdk`:

```
thread 'main' has overflowed its stack
fatal runtime error: stack overflow
```
```
Segmentation fault (core dumped)
```
Re-run your script with `--release`.