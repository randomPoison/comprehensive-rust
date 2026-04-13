---
minutes: 2
---

<!--
Copyright 2025 Google LLC
SPDX-License-Identifier: CC-BY-4.0
-->

# `with` as constructor

`with` as a constructor sets one value among a type while using default values
for the rest.

`with` as in "`<Type>` with specific setting."

```rust,compile_fail,editable
# // Copyright 2025 Google LLC
# // SPDX-License-Identifier: Apache-2.0
#
impl<T> Vec<T> {
    // Initializes memory for at least N elements, len is still 0.
    fn with_capacity(capacity: usize) -> Vec<T>;
}
```

<details>

- `with` can appear as a constructor prefix, most commonly when initializing
  heap memory for container types.

  In this case, it's distinct from `new` constructors because it specifies the
  value for something that is not usually cared about by API users.

</details>
