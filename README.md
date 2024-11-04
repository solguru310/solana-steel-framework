# solana-steel-framework
Steel is a new modular framework designed for building smart contracts on Solana.

## Contact me
If you need more technical support and development inquires, you can contact below.

Telegram: [@dwlee918](https://t.me/@dwlee918)

X: [@derricklee918](https://x.com/derricklee918)

Discord: [@solGuru](https://discordapp.com/users/352387576017190913)


## Overview
Steel allows developers to create programs with minimal boilerplate code while offering flexible functionality options. 
The framework aims to enhance contract performance, simplify the development process, and contribute positively to the Solana community.


## Getting started
To build with Steel, add it to your workspace dependencies:

```sh
cargo add steel
```

To compile your program, use the standard Solana toolchain:

```sh
cargo build-sbf
```

## API

Steel offers a collection of simple macros for defining the interface and building blocks of your program. 

### Accounts

For accounts, Steel uses a single enum to manage discriminators and a struct for each account type. The `account!` macro helps link these types and implements basic serialization logic.

```rs
use steel::*;

/// Enum for account discriminators.
#[repr(u8)]
#[derive(Clone, Copy, Debug, Eq, PartialEq, IntoPrimitive, TryFromPrimitive)]
pub enum MyAccount {
    Counter = 0,
}

/// Struct for account state.
#[repr(C)]
#[derive(Clone, Copy, Debug, PartialEq, Pod, Zeroable)]
pub struct Counter {
    pub value: u64,
}

account!(MyAccount, Counter);
```

### Instructions

For instructions, Steel similarly uses a single enum to manage discriminators and a struct for each instruction args type. The `instruction!` macro helps link these types and implement basic serialization logic.

```rs
use steel::*;

/// Enum for instruction discriminators.
#[repr(u8)]
#[derive(Clone, Copy, Debug, Eq, PartialEq, TryFromPrimitive)]
pub enum MyInstruction {
    Initialize = 0,
    Add = 1,
}

/// Struct for instruction args.
#[repr(C)]
#[derive(Clone, Copy, Debug, Pod, Zeroable)]
pub struct Initialize {}

/// Struct for instruction args.
#[repr(C)]
#[derive(Clone, Copy, Debug, Pod, Zeroable)]
pub struct Add {
    pub value: u64,
}

instruction!(MyInstruction, Initialize);
instruction!(MyInstruction, Add);

```

### Errors

Custom program errors can be created simply by defining an enum for your error messages and passing it to the `error!` macro. 

```rs
use steel::*;

/// Enum for error types.
#[repr(u32)]
#[derive(Debug, Error, Clone, Copy, PartialEq, Eq, IntoPrimitive)]
pub enum MyError {
    #[error("You did something wrong")]
    Dummy = 0,
}

error!(MyError);
```

### Events

Similarly, custom program events can be created by defining the event struct and passing it to the `event!` macro. 

```rs
use steel::*;

/// Struct for logged events.
#[repr(C)]
#[derive(Clone, Copy, Debug, PartialEq, Pod, Zeroable)]
pub struct MyEvent {
    pub value: u64,
}

event!(MyEvent);
```


## Program
In your contract implementation, Steel provides a set of composable functions to parse accounts, validate states, and execute Cross-Program Invocations (CPIs).

### Entrypoint
Steel includes a utility function to simplify the program entrypoint, securely parsing incoming instruction data and routing it to the appropriate handler.

### Validation
Steel offers a library of composable functions for validating account data. These functions can be chained together to validate arbitrary account states and convert them into any required format.

### CPIs
Steel provides a set of helper functions to simplify common CPIs, such as creating accounts, creating token accounts, minting tokens, burning tokens, and more.
