---
minutes: 5
---

<!--
Copyright 2025 Google LLC
SPDX-License-Identifier: CC-BY-4.0
-->

# From & Into

Conversion from one type to another.

Derivable: ❌, without crates like `derive_more`.

```rust,editable
# // Copyright 2025 Google LLC
# // SPDX-License-Identifier: Apache-2.0
#
pub struct ObviousImplementation(String);

impl From<String> for ObviousImplementation {
    fn from(value: String) -> Self {
        ObviousImplementation(value)
    }
}

impl From<&str> for ObviousImplementation {
    fn from(value: &str) -> Self {
        ObviousImplementation(value.to_owned())
    }
}

// `Into` is more natural to use as a trait bound.
fn into_string<S: Into<String>>(s: S) {}
fn string_from<T>(t: T) where String: From<T> {}

fn main() {
    let obvious1 = ObviousImplementation::from("Hello, obvious!".to_string());
    let obvious2 = ObviousImplementation::from("Hello, obvious!");

    // A From implementation implies an Into implementation, &str.into() ->
    // ObviousImplementation
    let obvious3: ObviousImplementation = "Hello, implementation!".into();
}
```

<details>

- Provides conversion functionality to types.

- The two traits exist to express different areas you'll find conversion in
  codebases.

- `From` provides a constructor-style function, whereas `Into` provides a method
  on an existing value.

- Prefer writing `From<T>` implementations for a type you're authoring instead
  of `Into<T>`.

  The `Into` trait is implemented for any type that implements `From`
  automatically.

  `Into` is preferred as a trait bound for arguments to functions for clarity of
  intent for what the function can take.

  `T: Into<String>` has clearer intent than `String: From<T>`.

</details>
