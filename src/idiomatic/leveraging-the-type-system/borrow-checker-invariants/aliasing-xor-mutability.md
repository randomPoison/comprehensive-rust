---
minutes: 15
---

<!--
Copyright 2025 Google LLC
SPDX-License-Identifier: CC-BY-4.0
-->

# Mutually Exclusive References / "Aliasing XOR Mutability"

We can use the mutual exclusion of `&mut T` references to prevent data
from being used before it is ready.

```rust,editable
# // Copyright 2025 Google LLC
# // SPDX-License-Identifier: Apache-2.0
#
pub struct QueryResult;
pub struct DatabaseConnection {/* fields omitted */}

impl DatabaseConnection {
    pub fn new() -> Self {
        Self {}
    }
    
    pub fn begin_transaction(&mut self) -> Transaction<'_> {
        Transaction { connection: self }
    }
}

pub struct Transaction<'a> {
    connection: &'a mut DatabaseConnection,
}

impl<'a> Transaction<'a> {
    pub fn query(&mut self, _query: &str) {
        // Send the query over, but don't wait for results.
    }

    pub fn commit(self) -> QueryResult {
        // Finish executing the transaction and retrieve the results.
        QueryResult
    }
}

fn main() {
    let mut db = DatabaseConnection::new();

    // The transaction `tx` mutably borrows `db`.
    let mut tx = db.begin_transaction();
    tx.query("SELECT * FROM users");

    // This won't compile because `db` is already mutably borrowed by `tx`.
    // let tx2 = db.begin_transaction(); // ❌🔨

    // The borrow of `db` ends when `tx` is consumed by `commit()`.
    let _result = tx.commit();

    // Once the first transaction finishes we can start another one.
    let _tx2 = db.begin_transaction();
}
```

<details>

- Motivation: We only want to support one in-flight transaction at a time.

- This example shows how we can use Aliasing XOR Mutability to prevent this kind
  of misuse.

- The constructor for the `Transaction` type takes a mutable reference to the
  database connection, and stores it in the returned `Transaction` value.

  The explicit lifetime here doesn't have to be intimidating, it just means
  "`Transaction` is outlived by the `DatabaseConnection` that was passed to it"
  in this case.

  The reference is mutable to completely lock out the `DatabaseConnection` from
  other usage, such as starting further transactions or reading the results.

- While a `Transaction` exists, we can't touch the `DatabaseConnection` variable
  that was created from it. Uncomment the line that tries to create a second
  transaction and show the resulting compiler error.

</details>
