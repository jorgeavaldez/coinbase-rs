# Coinbase Client for Rust

This is the a Rust client for interacting with the Coinbase (not Pro) API. It
works with version 2019-04-03 (v2) of the API.

## Features

- Sync and Async support
- Private and Public API

## Examples

Cargo.toml:

```toml
[dependencies]
coinbase-rs = "0.1.0"
```

### Private API (Sync)

```rust
use coinbase_rs::{Private, Sync, MAIN_URL};
use std::str::FromStr;
use uuid::Uuid;

pub const KEY: &str = "<put key here>";
pub const SECRET: &str = "<put secret here>";

fn main() {
    let client: Private<Sync> = Private::new(MAIN_URL, KEY, SECRET);

    let accounts = client.accounts().unwrap();
    for account in accounts {
        println!("Account {}", account.currency.code);
        if let Ok(id) = Uuid::from_str(&account.id) {
            for transaction in client.list_transactions(&id).unwrap() {
                println!(
                    "Transaction {} = {}",
                    transaction.id, transaction.amount.amount
                );
            }
        }
    }
}
```

## Thanks

This project is inspired and borrows heavily from
[coinbase-pro-rs](https://github.com/inv2004/coinbase-pro-rs).