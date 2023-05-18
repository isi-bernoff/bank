# Bank
This project is a recreation of [UMass](https://www.umass.edu)'s [Operating Systems](https://www.cics.umass.edu/content/spring-23-course-descriptions#377) TAs [Dave Dirnfield](https://github.com/dd2912) and [Calvin Chai](https://github.com/NightDawnEX)'s C/C++ [Producer/Consumer](https://github.com/umass-cs-377/umass-cs-377.github.io/blob/a18ded192a18bc59affecb8ef077849617b3a61d/docs/04-projects/04-prodcon/index.md) project into [Rust](https://www.https://www.rust-lang.org). It is a banking application that uses threads to process transactions from a ledger file concurrently. Watch my presentation [on Google Drive here](https://drive.google.com/file/d/1JzMJABKetQsdgeSP7Np1wbjC1PZ1y4N7/view?usp=sharing).
## Prerequisites
Rust 1.65.0+
## Execution
```console
$ cargo run <num_threads> <ledger_filepath>
```
## Structures
### `Transaction`
#### Fields
- `id`: The `u16` identifier for this `Transaction`
- `from_id`: The `u16` identifier of the account having its money removed
- `to_id`: The `u16` identifier of the account having receiving money
- `amount`: The `f32` amount of money being moved
- `mode`: The `enum Mode` of transaction, a `Deposit`, `Withdrawal`, or `Transfer`
### `Bank`
#### Fields
- `accounts`: `BTreeMap` of `u16` identifiers to `Mutex`-locked `f32` balances
- `ledger`: A `Vec` of `Transactions`
- `num_transactions`: The `u16` number of `Transaction`s in the `ledger`
- `num_succeeded`: The `u16` number of `Transaction`s that succeeded
## Methods
```rs
Transaction::new(from_id: u16, to_id: u16, amount: f32, mode_id: u8) -> Transaction
```
Constructs a new `Bank` object and initializes its `accounts`, `ledger`, `num_transactions`, and `num_succeeded`
#### Parameters
- `num_accounts`: The number of `account`s to initialize
- `ledger_filepath`: The name of a ledger file containing transactions formatted `<from_id>
  <to_id> <amount> <mode_num>` on each line
#### Returns
The new `Bank` object
#### Returns
The new `Transaction` object
```rs
Bank::new(num_accounts: u16, ledger_filepath: String) -> Bank
```
Constructs a new `Bank` object and initializes its `accounts`, `ledger`, `num_successful`, and `num_failed`
#### Parameters
- `num_accounts`: The number of `account`s to initialize
- `ledger_filepath`: The name of a ledger file containing transactions formatted `<from_id> <to_id> <amount> <mode_id>` on each line
#### Returns
The new `Bank` object
```rs
Bank::start(&mut self, num_threads: u16)
```
Spawns `Threads` to process this `Bank`'s `ledger` concurrently
#### Parameters
- `num_threads`: The number of `Thread`s to spawn
- `arc_bank`: The atomic reference counter to this `Mutex`-locked `Bank`
```rs
Bank::process_transaction(arc_bank: Arc<Mutex<Bank>>, thread_id: u16)
```
Deposit money into one of this `Bank`'s `accounts`, incrementing `num_succeeded` if the
deposit worked
#### Parameters
- `account_id`: The identifier of the account receiving the deposit
- `amount`: The amount of money being deposited
#### Returns
`true` if the deposit succeeds and `false` otherwise
```rs
Bank::deposit(&mut self, account_id: u16, amount: f32) -> bool
```
Deposit money into one of this `Bank`'s `accounts`, incrementing
`num_successful` or `num_failed` depending on if the deposit works
#### Parameters
- `thread_id`: The identifier of the `Thread` processing the the deposit
- `account_id`: The identifier of the account receiving the deposit
- `amount`: The amount of money being deposited
#### Returns
`true` if the deposit succeeds and `false` otherwise
```rs
Bank::withdraw(&mut self, account_id: u16, amount: f32) -> bool
```
Withdraws money from one of this `Bank`'s `accounts`, incrementing `num_succeeded` if the
withdrawal worked
#### Parameters
- `account_id`: The identifier of the account having its money withdrawn
- `amount`: The amount of money being withdrawn
#### Returns
`true` if the withdrawal succeeds and `false` otherwise
```rs
Bank::transfer(&mut self, from_id: u16, to_id: u16, amount: f32) -> bool
```
Transfers money from one of this `Bank`'s `accounts` to another, incrementing
`num_succeeded` if the transfer worked
#### Parameters
- `from_id`: The identifier of the account having money transferred from it
- `to_id`: The identifier of the account having money transferred to it
- `amount`: The amount of money being transferred
#### Returns
`true` if the transfer succeeds and `false` otherwise

##### Thanks to recent UMass MS graduate [Ryan Lee](https://github.com/rlee287) for ~~guarding~~ guiding me through the idiosyncrasies of Rust.