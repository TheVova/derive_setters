# derive_setters

[![Build Status](https://api.travis-ci.org/Lymia/derive_setters.svg?branch=master)](https://travis-ci.org/Lymia/derive_setters)
[![Latest Version](https://img.shields.io/crates/v/derive_setters.svg)](https://crates.io/crates/derive_setters)
![Requires rustc 1.31+](https://img.shields.io/badge/rustc-1.31+-red.svg)

Rust macro to automatically generates setter methods for a struct's fields. This can be used to add setters to a plain
data struct, or to help in implementing builders.

For a related library that creates separate builder types, see 
[`rust-derive-builder`](https://github.com/colin-kiegel/rust-derive-builder).

# Basic usage example

```rust
use derive_setters::*;

#[derive(Default, Setters, Debug, PartialEq, Eq)]
struct BasicStruct {
    #[setters(rename = "test")]
    a: u32,
    b: u32,
    c: u32,
}

assert_eq!(
    BasicStruct::default().test(30).b(10).c(20),
    BasicStruct { a: 30, b: 10, c: 20 },
);
```

# Additional options

The following options can be set on the entire struct.

* `#[setters(generate = "false")]` causes setter methods to not be generated by default.
* `#[setters(generate_private = "false")]` causes setter methods to not be generated by default for private fields.
* `#[setters(generate_public = "false")]` causes setter methods to not be generated by default for public fields.
* `#[setters(no_std)]` causes the generated code to use `core` instead of `std`.
* `#[setters(prefix = "with_")]` causes the setter method on all fields to be prefixed with the given string.
* `#[setters(generate_delegates(ty = "OtherTy", field = "some_field"))]` causes all setter methods on this struct to
  be duplicated on the target struct, instead modifying `self.some_field` instead of `self`.
* `#[setters(generate_delegates(ty = "OtherTy", method = "get_field"))]` does the same thing as above, except calling
  `get_field` with no arguments instead of directly accessing a field.

The following options can be set on a fields.

* `#[setters(generate)]` causes a setter method to be generated, overriding struct-level settings.
* `#[setters(skip)]` causes no setter method to be generated.
* `#[setters(rename = "setter_name")]` causes the setter method to have a different name from the field.
   This overwrites `add_prefix`.

The following options can be set on either the entire struct, or on an individual field. You
can disable the features for a particular field with `#[setters(option = "false")]`

* `#[setters(into)]` causes the setter method to accept any type that can be converted into the field's type
  via `Into`.
* `#[setters(strip_option)]` causes the setter method to accept `T` instead of `Option<T>`. If applied to a field
  that isn't wrapped in an `Option`, it does nothing.
* `#[setters(bool)]` causes the setter method to take no arguments, and set the field to `true`.

# License

This project is licensed under either of

 * Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or
   http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or
   http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in enumset by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
