# Get User Positions

Retrieves all lending positions for a specific user address on a given chain.

## Endpoint

```
GET /v2/lending/{chainId}/users/{userAddress}/positions
```

## Base URL

```
https://api.fluid.instadapp.io
```

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `chainId` | integer | Yes | The chain ID (e.g., `1` for Ethereum mainnet) |
| `userAddress` | string | Yes | The user's wallet address (must be a valid Ethereum address) |

## Example Request

```bash
curl -X GET "https://api.fluid.instadapp.io/v2/lending/1/users/0xc1490E0489f487477A9B4e52Da19416d21fC09E0/positions"
```

## Response

Returns an array of position objects, each representing a user's position in a specific Fluid token.

### Response Structure

```json
{
  "data": [
    {
      "token": { ... },
      "shares": "string",
      "underlyingAssets": "string",
      "underlyingBalance": "string",
      "allowance": "string",
      "userRewards": { ... },
      "tokenRewards": { ... },
      "totalShares": "string",
      "totalUnderlyingAssets": "string"
    }
  ]
}
```

### Response Fields

#### Root Level

| Field | Type | Description |
|-------|------|-------------|
| `data` | array | Array of position objects for each available token |

#### Position Object

| Field | Type | Description |
|-------|------|-------------|
| `token` | object | Token information and metadata (see [Token Object](#token-object)) |
| `shares` | string | User's current share balance in the token (in token decimals) |
| `underlyingAssets` | string | User's underlying asset balance (in asset decimals) |
| `underlyingBalance` | string | User's underlying token balance in their wallet (in asset decimals) |
| `allowance` | string | User's token allowance for the protocol (in token decimals) |
| `userRewards` | object | User's reward information (see [User Rewards Object](#user-rewards-object)) |
| `tokenRewards` | object | Token reward contract information (see [Token Rewards Object](#token-rewards-object)) |
| `totalShares` | string | Total shares held by the user across all positions (in token decimals) |
| `totalUnderlyingAssets` | string | Total underlying assets held by the user (in asset decimals) |

#### Token Object

| Field | Type | Description |
|-------|------|-------------|
| `address` | string | Contract address of the Fluid token |
| `eip2612Deposits` | boolean | Whether the token supports EIP-2612 permit-based deposits |
| `isNativeUnderlying` | boolean | Whether the underlying asset is native (e.g., ETH) |
| `name` | string | Full name of the Fluid token |
| `symbol` | string | Token symbol (e.g., `fUSDC`, `fWETH`) |
| `decimals` | integer | Number of decimals for the token |
| `assetAddress` | string | Contract address of the underlying asset |
| `rewards` | array | Array of reward programs available for this token (see [Reward Object](#reward-object)) |
| `asset` | object | Underlying asset information (see [Asset Object](#asset-object)) |
| `totalAssets` | string | Total assets in the pool (in asset decimals) |
| `totalSupply` | string | Total supply of Fluid tokens (in token decimals) |
| `convertToShares` | string | Conversion rate from assets to shares (multiplier, e.g., `848772` = 0.848772) |
| `convertToAssets` | string | Conversion rate from shares to assets (multiplier, e.g., `1178172` = 1.178172) |
| `rewardsRate` | string | Additional rewards APR (basis points, e.g., `0` = 0%) |
| `supplyRate` | string | Supply interest rate APR (basis points, e.g., `390` = 3.90%) |
| `totalRate` | string | Total APR including supply rate and rewards (basis points) |
| `liquiditySupplyData` | object | Liquidity and withdrawal information (see [Liquidity Supply Data Object](#liquidity-supply-data-object)) |

#### Asset Object

| Field | Type | Description |
|-------|------|-------------|
| `address` | string | Contract address of the asset |
| `name` | string | Full name of the asset |
| `symbol` | string | Asset symbol (e.g., `USDC`, `WETH`) |
| `decimals` | integer | Number of decimals for the asset |
| `price` | string | Current price of the asset in USD |
| `chainId` | string | Chain ID where the asset exists |
| `logoUrl` | string | URL to the asset's logo image |
| `coingeckoId` | string | CoinGecko identifier for the asset |
| `updatedAt` | string | ISO 8601 timestamp of when the price was last updated |
| `stakingApr` | string | (Optional) Staking APR for the asset (basis points, e.g., `425` = 4.25%) |

#### Reward Object

| Field | Type | Description |
|-------|------|-------------|
| `token` | object | Reward token information (see [Asset Object](#asset-object)) |
| `rate` | string | Reward rate APR (basis points, e.g., `149` = 1.49%) |
| `endTime` | integer | Unix timestamp (milliseconds) when the reward program ends |
| `rewardType` | string | Type of reward program (`merkle` or other) |
| `type` | string | (Optional) Additional reward type classification (e.g., `supply`) |
| `metadata` | object | Reward program metadata (see [Reward Metadata Object](#reward-metadata-object)) |

#### Reward Metadata Object

| Field | Type | Description |
|-------|------|-------------|
| `merkle` | object | (Optional) Merkle reward program details |
| `claimUrl` | string | (Optional) URL to claim rewards (for non-merkle rewards) |

##### Merkle Object

| Field | Type | Description |
|-------|------|-------------|
| `programId` | string | Identifier for the reward program |
| `merkleContract` | string | Contract address of the Merkle distributor |

#### Liquidity Supply Data Object

| Field | Type | Description |
|-------|------|-------------|
| `modeWithInterest` | boolean | Whether the pool operates with interest accrual |
| `supply` | string | Total supply in the pool (in asset decimals) |
| `withdrawalLimit` | string | Current withdrawal limit (in asset decimals, `0` means no limit) |
| `lastUpdateTimestamp` | string | Unix timestamp of the last update |
| `expandPercent` | string | Percentage by which the withdrawal limit can expand (basis points, e.g., `5000` = 50%) |
| `expandDuration` | string | Duration in seconds for withdrawal limit expansion |
| `baseWithdrawalLimit` | string | Base withdrawal limit before expansion (in asset decimals) |
| `withdrawableUntilLimit` | string | Maximum withdrawable amount considering the limit (in asset decimals) |
| `withdrawable` | string | Currently available withdrawable amount (in asset decimals) |

#### User Rewards Object

| Field | Type | Description |
|-------|------|-------------|
| `earned` | string | Total rewards earned by the user (in reward token decimals) |
| `tokenShares` | string | User's reward token shares (in token decimals) |
| `underlyingAssets` | string | User's underlying assets from rewards (in asset decimals) |
| `tokenAllowance` | string | User's allowance for reward token (in token decimals) |

#### Token Rewards Object

| Field | Type | Description |
|-------|------|-------------|
| `rewardContract` | string | Contract address of the reward distributor (zero address if no rewards) |
| `rewardPerToken` | string | Cumulative reward per token (in reward token decimals) |
| `getRewardForDuration` | string | Total rewards available for the full duration (in reward token decimals) |
| `totalSupply` | string | Total supply of tokens staked in the reward contract (in token decimals) |
| `periodFinish` | string | Unix timestamp when the reward period ends |
| `rewardRate` | string | Rate at which rewards are distributed per second (in reward token decimals) |
| `rewardsDuration` | string | Duration of the reward period in seconds |
| `rewardsToken` | string | Contract address of the reward token |
| `token` | string | Contract address of the staked token |
| `apr` | string | Annual percentage rate for rewards (basis points) |

## Example Response

```json
{
  "data": [
    {
      "token": {
        "address": "0x9Fb7b4477576Fe5B32be4C1843aFB1e55F251B33",
        "eip2612Deposits": true,
        "isNativeUnderlying": false,
        "name": "Fluid USD Coin",
        "symbol": "fUSDC",
        "decimals": 6,
        "assetAddress": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
        "rewards": [
          {
            "token": {
              "address": "0x6f40d4a6237c257fff2db00fa0510deeecd303eb",
              "name": "Fluid",
              "symbol": "FLUID",
              "decimals": 18,
              "price": "3.267020355727",
              "chainId": "1",
              "logoUrl": "https://coin-images.coingecko.com/coins/images/14688/large/Logo_1_%28brighter%29.png?1734430693",
              "coingeckoId": "instadapp",
              "updatedAt": "2025-12-12T12:51:02.000+00:00"
            },
            "rate": "149",
            "endTime": 1767883500000,
            "rewardType": "merkle",
            "metadata": {
              "merkle": {
                "programId": "inst-rewards-dec-2024",
                "merkleContract": "0x7060FE0Dd3E31be01EFAc6B28C8D38018fD163B0"
              }
            }
          }
        ],
        "asset": {
          "address": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
          "name": "USD Coin",
          "symbol": "USDC",
          "decimals": 6,
          "price": "0.999859258573",
          "chainId": "1",
          "logoUrl": "https://coin-images.coingecko.com/coins/images/6319/large/usdc.png?1696506694",
          "coingeckoId": "usd-coin",
          "updatedAt": "2025-12-12T12:54:02.000+00:00"
        },
        "totalAssets": "266957084195625",
        "totalSupply": "226585770240645",
        "convertToShares": "848772",
        "convertToAssets": "1178172",
        "rewardsRate": "0",
        "supplyRate": "390",
        "totalRate": "390",
        "liquiditySupplyData": {
          "modeWithInterest": true,
          "supply": "266957084195635",
          "withdrawalLimit": "133478542097817",
          "lastUpdateTimestamp": "1765542287",
          "expandPercent": "5000",
          "expandDuration": "21600",
          "baseWithdrawalLimit": "10219069688577",
          "withdrawableUntilLimit": "133478542097818",
          "withdrawable": "69024941342225"
        }
      },
      "shares": "0",
      "underlyingAssets": "0",
      "underlyingBalance": "0",
      "allowance": "0",
      "userRewards": {
        "earned": "0",
        "tokenShares": "0",
        "underlyingAssets": "0",
        "tokenAllowance": "0"
      },
      "tokenRewards": {
        "rewardContract": "0x2fA6c95B69c10f9F52b8990b6C03171F13C46225",
        "rewardPerToken": "26112841441402330203196120613",
        "getRewardForDuration": "249999999999999996672000",
        "totalSupply": "1115547530102",
        "periodFinish": "1716393683",
        "rewardRate": "32150205761316872",
        "rewardsDuration": "7776000",
        "rewardsToken": "0x6f40d4A6237C257fff2dB00FA0510DeEECd303eb",
        "token": "0x9Fb7b4477576Fe5B32be4C1843aFB1e55F251B33",
        "apr": "0"
      },
      "totalShares": "0",
      "totalUnderlyingAssets": "0"
    }
  ]
}
```

## Notes

- All numeric values are returned as strings to avoid precision loss with large numbers
- Rates are provided in basis points (1 basis point = 0.01%)
- Timestamps are in Unix format (seconds or milliseconds as indicated)
- The `convertToShares` and `convertToAssets` values are multipliers (divide by 1,000,000 for the actual rate)
- A `withdrawalLimit` of `0` indicates no withdrawal limit is currently active
- Zero addresses (`0x0000000000000000000000000000000000000000`) indicate that a feature is not available for that token
- The response includes all available tokens, even if the user has no position (shares = 0)

## Error Responses

The API may return standard HTTP error codes:

- `400 Bad Request` - Invalid parameters (e.g., invalid user address format)
- `404 Not Found` - Chain ID or endpoint not found
- `500 Internal Server Error` - Server error

