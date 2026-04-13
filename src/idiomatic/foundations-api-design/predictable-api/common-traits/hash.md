---
minutes: 2
---

<!--
Copyright 2025 Google LLC
SPDX-License-Identifier: CC-BY-4.0
-->

# Hash

Performing a hash on a type.

Derivable: ✅

```rust,editable
# // Copyright 2025 Google LLC
# // SPDX-License-Identifier: Apache-2.0
#
#[derive(Hash)]
pub struct User {
    id: u32,
    name: String,
    friends: Vec<u32>,
}
```

<details>

- Allows a type to be used in hash algorithms.

- Most commonly used with data structures like `HashMap`.

- `Hash` doesn't define any of the hashing logic itself, instead it just feeds
  the type's data into a `Hasher`. This allows us to use different hash
  algorithms without changing a type's `Hash` impl.

</details>
