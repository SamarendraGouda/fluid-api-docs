## Withdraw Action

### Function: `withdraw()`

Withdraws tokens from a lending position. Supports both ERC20 and native tokens.

1. **ERC20 Tokens & Native Tokens**:

   - If withdraw max amount: Calls `redeem` function
   - If withdraw any other amount: Calls `withdraw` function
   - Transaction data:

     ```typescript
     // For max amount (redeem)
     {
       to: fTokenAddress,
       data: encodeFunctionData({
         abi: ERC20Abi,
         functionName: 'redeem',
         args: [shares, receiver.address, owner.address]
       })
     }

     // For specific amount (withdraw)
     {
       to: fTokenAddress,
       data: encodeFunctionData({
         abi: ERC20Abi,
         functionName: 'withdraw',
         args: [BigInt(withdrawAmountInWei), receiver.address, owner.address]
       })
     }
     ```

   - ERC20 ABI:
     ```typescript
      const ERC20Abi = 
      // Withdraw function  
     {
     inputs: [
      {
        internalType: 'uint256',
        name: 'assets_',
        type: 'uint256',
      },
      {
        internalType: 'address',
        name: 'receiver_',
        type: 'address',
      },
      {
        internalType: 'address',
        name: 'owner_',
        type: 'address',
      },
     ],
     name: 'withdraw',
     outputs: [
      {
        internalType: 'uint256',
        name: 'shares_',
        type: 'uint256',
      },
     ],
     stateMutability: 'nonpayable',
     type: 'function',
     },

     // Redeem Function
     {
     inputs: [
      {
        internalType: 'uint256',
        name: 'shares_',
        type: 'uint256',
      },
      {
        internalType: 'address',
        name: 'receiver_',
        type: 'address',
      },
      {
        internalType: 'address',
        name: 'owner_',
        type: 'address',
      },
     ],
     name: 'redeem',
     outputs: [
      {
        internalType: 'uint256',
        name: 'assets_',
        type: 'uint256',
      },
     ],
     stateMutability: 'nonpayable',
     type: 'function',
     }
     ```

**Flow:**

```

User → withdraw() / redeem() → transaction sent
```
