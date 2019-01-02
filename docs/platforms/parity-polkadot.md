# Parity and Polkadot

## Overview

* "Substrate combines three new technologies: Rust, WebAssembly and Libp2p in order to create a framework that makes building upgradable and efficient."
* Parity originally created Polkadot with the intention of supporting Wasm smart contracts authored in Rust. The Polkadot codebase was recently split and the foundation code for Wasm contract support has been moved into the Substrate codebase. 


## Links
* https://substrate.readme.io/
* https://wiki.parity.io/
* Evaluate: https://github.com/paritytech/parity-wasm
* [Github: Substrate](https://github.com/paritytech/substrate)
* [Why we believe in Wasm as the base layer of decentralised application development](https://www.parity.io/wasm-smart-contract-development/)
* [Parity WebAssembly Links](https://wiki.parity.io/WebAssembly-Links.html)


## Contract Examples

```rust
// Source: https://github.com/paritytech/pwasm-tutorial/blob/master/step-2/src/lib.rs

extern crate pwasm_std;
extern crate pwasm_ethereum;
extern crate pwasm_abi;
extern crate pwasm_abi_derive;
/// Bigint used for 256-bit arithmetic

pub mod token {
    use pwasm_ethereum;
    use pwasm_abi::types::*;

    // eth_abi is a procedural macros https://doc.rust-lang.org/book/first-edition/procedural-macros.html
    use pwasm_abi_derive::eth_abi;

    static TOTAL_SUPPLY_KEY: H256 = H256([2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]);

    #[eth_abi(TokenEndpoint)]
    pub trait TokenInterface {
        /// The constructor
        fn constructor(&mut self, _total_supply: U256);
        /// Total amount of tokens
        fn totalSupply(&mut self) -> U256;
    }

    pub struct TokenContract;

    impl TokenInterface for TokenContract {
        fn constructor(&mut self, total_supply: U256) {
            // Set up the total supply for the token
            pwasm_ethereum::write(&TOTAL_SUPPLY_KEY, &total_supply.into());
        }

        fn totalSupply(&mut self) -> U256 {
            U256::from_big_endian(&pwasm_ethereum::read(&TOTAL_SUPPLY_KEY))
        }
    }
}
```
<!-- [filename](https://github.com/paritytech/pwasm-tutorial/blob/master/step-2/src/lib.rs ':include') -->