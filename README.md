Sorgo Savings

Sorgo Savings is an open-source, standardized yield-routing vault built on Stellar using Soroban smart contracts. It provides a plug-and-play architecture for routing stablecoin deposits (like USDC) across multiple DeFi yield sources without needing to write custom, unaudited integration code for every protocol.

🎯 The Problem

Stellar's DeFi TVL is growing rapidly with protocols like Blend and Aquarius live on the network. However, capital often stays siloed in single pools because builders lack an open adapter standard. Currently, any team wanting to route user funds to a yield source must write their own custom integration code from scratch.

💡 The Solution

Sorgo Savings introduces a reusable Vault Core and a Strategy Adapter Trait. By decoupling the vault accounting from the specific yield protocol integrations, other developers can safely and easily add new yield sources without altering the core vault logic.

Core Components

Vault Core: A Soroban smart contract that accepts USDC deposits, mints yield-bearing shares for users, and enforces per-strategy allocation caps.

Strategy Adapter Trait: A standardized Rust trait defining the required interface for any yield source:

deposit()

withdraw()

current_value()

Protocol Adapters: Individual smart contracts implementing the Strategy Adapter Trait. (Currently features a live integration for the Blend protocol).

🚀 Getting Started (Testnet)

Prerequisites

Rust

Soroban CLI

A Stellar Testnet account funded with testnet XLM.

Build and Test

Clone the repository and build the contracts:

git clone https://github.com/your-org/sorgo-savings.git
cd sorgo-savings
soroban contract build


Run the unit and integration tests (includes mock Blend protocol tests):

cargo test


🛠 Writing a New Adapter

Sorgo is designed to be extensible. To add support for a new yield protocol (e.g., Aquarius), you simply need to create a new contract that implements the StrategyAdapter trait.

pub trait StrategyAdapter {
    // Invest funds into the underlying protocol
    fn deposit(env: Env, amount: i128) -> Result<(), Error>;
    
    // Withdraw funds from the underlying protocol
    fn withdraw(env: Env, amount: i128) -> Result<(), Error>;
    
    // Return the current value of the assets managed by this adapter
    fn current_value(env: Env) -> i128;
}


A full integration guide and tutorial video will be available soon.

📦 Current Adapters

Blend Protocol (Testnet): Working adapter routing testnet USDC into Blend's lending pools.

🛣 Roadmap

This project is currently in a 30-day sprint funded by a Stellar Instaward.

Current Sprint Goals:

Finalize Core Vault math and share accounting.

Deploy Core Vault and Blend Adapter to Stellar Testnet.

Publish a 3-minute video demo of the deposit -> allocate -> value read flow.

Future Considerations:

Automated rebalancing logic.

Mainnet deployment and professional audits.

Additional adapters for other Stellar DeFi protocols.

📄 License

This project is licensed under the MIT License.
