# README

## Overview

This repository contains two Solidity smart contracts designed for the V4 liquidity pool framework. The contracts aim to enhance the security of swap operations within the pool, mitigating specific types of attacks, such as excessive swapping and sandwich attacks.

### Contracts

1. **SimpleSandwichDefender**
2. **OrdersLimitDefender**

---

## 1. SimpleSandwichDefender

### Purpose

The **SimpleSandwichDefender** contract prevents users from performing multiple swaps within the same block. This is aimed at mitigating sandwich attack vulnerabilities, where malicious actors could exploit the timing of transactions for profit.

### Key Features

- **Block-Level Swap Protection:** 
  - Tracks the last block number a user interacted with for each pool to prevent actions within the same block.
  
### Functions

- **getHookPermissions:** 
  - Returns permissions for hooks, allowing the `beforeSwap` hook for monitoring swap interactions.

- **beforeSwap:** 
  - Validates that the user has not performed a swap in the same block, logging the swap origin and sender before updating the last swap block.

### Usage

Deploy the contract with an instance of `IPoolManager`. This contract will actively monitor and protect against potential sandwich attacks by ensuring that users cannot initiate multiple swaps in a single block.

---

## 2. OrdersLimitDefender

### Purpose

The **OrdersLimitDefender** contract restricts the number of swaps a user can perform within a one-minute timeframe, enforcing a limit of five swaps per minute. This helps to prevent abuse of the swap functionality and ensures fair usage of the liquidity pool.

### Key Features

- **User Swap Tracking:** 
  - Maintains a record of the number of swaps and timestamps for each user within each pool.
  
- **Swap Count Enforcement:** 
  - Ensures that a user cannot exceed the limit of five swaps in any given minute.
  
- **Timestamp Cleanup:**
  - Old timestamps are cleaned periodically to maintain an accurate count of swaps in the last minute.

### Functions

- **getHookPermissions:** 
  - Returns permissions for various hooks, enabling the `beforeSwap` hook for managing swap limits.

- **beforeSwap:** 
  - Checks the user's swap count, increments it, and returns the appropriate swap delta values.

- **cleanOldTimestamps:** 
  - Internal function to remove timestamps that are older than one minute from the user's swap history.

### Usage

Deploy the contract by providing an instance of the `IPoolManager`. Users will be monitored for their swap actions in specified liquidity pools.

---

## Conclusion

Both contracts serve essential roles in maintaining the integrity and fairness of the swap operations within liquidity pools. By implementing user-specific restrictions and monitoring, they contribute to a more secure trading environment.

For further integration or testing, please refer to the relevant documentation for the V4 liquidity pool framework and its associated libraries.
