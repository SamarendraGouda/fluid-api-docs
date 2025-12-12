## Deposit Action

### Function: `deposit()`

Deposits tokens into a lending position. Supports both ERC20 and native tokens.

1. **ERC20 Tokens**:

   - If approval is needed, the user must call `approve()` separately first
   - Then sends a single `deposit` transaction to the position token contract
   - Transaction data:
     ```typescript
     {
       to: fTokenAddress,
       data: encodeFunctionData({
         abi: ERC20Abi,
         functionName: 'deposit',
         args: [BigInt(depositAmountInWei), userAddress]
       })
     }
     ```
   - ERC20 ABI:
     ```typescript
      const ERC20Abi = {
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
            ],
            name: 'deposit',
            outputs: [
                {
                    internalType: 'uint256',
                    name: 'shares_',
                    type: 'uint256',
                },
            ],
            stateMutability: 'nonpayable',
            type: 'function',
        }

2. **Native Tokens**:
   - Sends a single `depositNative` transaction with native value
   - Transaction data:
     ```typescript
     {
       to: fTokenAddress,
       data: encodeFunctionData({
         abi: nativeAbi,
         functionName: 'depositNative',
         args: [userAddress]
       }),
       value: BigInt(depositAmountInWei)
     }
     ```
   - Native ABI:
     ```typescript
        const nativeAbi = {
            inputs: [
                {
                    internalType: "address",
                    name: "receiver_",
                    type: "address",
                },
            ],
            name: "depositNative",
            outputs: [
                {
                    internalType: "uint256",
                    name: "shares_",
                    type: "uint256",
                },
            ],
            stateMutability: "payable",
            type: "function",
        };
     ```

**Flow:**
```

User → approve() [if needed] → deposit() → Transaction sent

````
