# Get Lending Tokens

Retrieves all available lending tokens and their current market data for a specific chain.

## Endpoint

```
GET /v2/lending/{chainId}/tokens
```

## Base URL

```
https://api.fluid.instadapp.io
```

## Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `chainId` | integer | Yes | The chain ID (e.g., `1` for Ethereum mainnet) |

## Example Request

```bash
curl -X GET "https://api.fluid.instadapp.io/v2/lending/1/tokens"
```

## Response

Returns an object containing aggregate statistics and an array of all available Fluid lending tokens with their current market data, rates, and liquidity information.

### Response Structure

```json
{
  "totalAssetsInUsd": "string",
  "yearlyYieldInUsd": "string",
  "data": [
    {
      "address": "string",
      "eip2612Deposits": boolean,
      "isNativeUnderlying": boolean,
      "name": "string",
      "symbol": "string",
      "decimals": integer,
      "assetAddress": "string",
      "rewards": [ ... ],
      "asset": { ... },
      "totalAssets": "string",
      "totalSupply": "string",
      "convertToShares": "string",
      "convertToAssets": "string",
      "rewardsRate": "string",
      "supplyRate": "string",
      "totalRate": "string",
      "liquiditySupplyData": { ... }
    }
  ]
}
```

### Response Fields

#### Root Level

| Field | Type | Description |
|-------|------|-------------|
| `totalAssetsInUsd` | string | Total value of all assets across all tokens in USD |
| `yearlyYieldInUsd` | string | Total yearly yield across all tokens in USD |
| `data` | array | Array of token objects (see [Token Object](#token-object)) |

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
| `stakingApr` | string | (Optional) Staking APR for the asset (basis points, e.g., `254` = 2.54%) |

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
| `merkle` | object | (Optional) Merkle reward program details (see [Merkle Object](#merkle-object)) |
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

## Example Response

```json
{
  "totalAssetsInUsd": "599391140.52",
  "yearlyYieldInUsd": "0.00",
  "data": [
    {
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
      "totalAssets": "266957096108825",
      "totalSupply": "226585770240645",
      "convertToShares": "848772",
      "convertToAssets": "1178172",
      "rewardsRate": "0",
      "supplyRate": "390",
      "totalRate": "390",
      "liquiditySupplyData": {
        "modeWithInterest": true,
        "supply": "266957096109041",
        "withdrawalLimit": "133478548054520",
        "lastUpdateTimestamp": "1765542287",
        "expandPercent": "5000",
        "expandDuration": "21600",
        "baseWithdrawalLimit": "10219070144620",
        "withdrawableUntilLimit": "133478548054521",
        "withdrawable": "69024123961553"
      }
    },
    {
      "address": "0x5C20B550819128074FD538Edf79791733ccEdd18",
      "eip2612Deposits": false,
      "isNativeUnderlying": false,
      "name": "Fluid Tether USD",
      "symbol": "fUSDT",
      "decimals": 6,
      "assetAddress": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
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
          "rate": "157",
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
        "address": "0xdac17f958d2ee523a2206206994597c13d831ec7",
        "name": "Tether USD",
        "symbol": "USDT",
        "decimals": 6,
        "price": "1.000164352728",
        "chainId": "1",
        "logoUrl": "https://coin-images.coingecko.com/coins/images/325/large/Tether.png?1696501661",
        "coingeckoId": "tether",
        "updatedAt": "2025-12-12T12:54:02.000+00:00"
      },
      "totalAssets": "252536124265834",
      "totalSupply": "215240963442382",
      "convertToShares": "852317",
      "convertToAssets": "1173271",
      "rewardsRate": "0",
      "supplyRate": "325",
      "totalRate": "325",
      "liquiditySupplyData": {
        "modeWithInterest": true,
        "supply": "252536124265854",
        "withdrawalLimit": "126268062132927",
        "lastUpdateTimestamp": "1765544135",
        "expandPercent": "5000",
        "expandDuration": "21600",
        "baseWithdrawalLimit": "10211471460879",
        "withdrawableUntilLimit": "126268062132927",
        "withdrawable": "96384250349818"
      }
    },
    {
      "address": "0x6A29A46E21C730DcA1d8b23d637c101cec605C5B",
      "eip2612Deposits": true,
      "isNativeUnderlying": false,
      "name": "Fluid Gho Token",
      "symbol": "fGHO",
      "decimals": 18,
      "assetAddress": "0x40D16FC0246aD3160Ccc09B8D0D3A2cD28aE6C2f",
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
          "rate": "135",
          "endTime": 1767883500000,
          "rewardType": "merkle",
          "metadata": {
            "merkle": {
              "programId": "inst-rewards-dec-2024",
              "merkleContract": "0x7060FE0Dd3E31be01EFAc6B28C8D38018fD163B0"
            }
          }
        },
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
          "rate": "26",
          "endTime": 1797863100000,
          "rewardType": "merkle",
          "metadata": {
            "merkle": {
              "programId": "mainnet-gho-vaults-rewards-april-2025",
              "merkleContract": "0xD833484b198D3d05707832cc1C2D62b520D95B8A"
            }
          }
        }
      ],
      "asset": {
        "address": "0x40d16fc0246ad3160ccc09b8d0d3a2cd28ae6c2f",
        "name": "Gho Token",
        "symbol": "GHO",
        "decimals": 18,
        "price": "1.000354161629",
        "chainId": "1",
        "logoUrl": "https://coin-images.coingecko.com/coins/images/30663/large/gho-token-logo.png?1720517092",
        "coingeckoId": "gho",
        "updatedAt": "2025-12-12T12:54:02.000+00:00"
      },
      "totalAssets": "59092891800252635028929126",
      "totalSupply": "54214499713676521365070842",
      "convertToShares": "917445365458400912",
      "convertToAssets": "1089983161559000000",
      "rewardsRate": "0",
      "supplyRate": "397",
      "totalRate": "397",
      "liquiditySupplyData": {
        "modeWithInterest": true,
        "supply": "59092891800251499612490396",
        "withdrawalLimit": "29546445900125749806245198",
        "lastUpdateTimestamp": "1765530203",
        "expandPercent": "5000",
        "expandDuration": "21600",
        "baseWithdrawalLimit": "10317913866958829176634876",
        "withdrawableUntilLimit": "29546445900125749806245198",
        "withdrawable": "20167396469565070586629010"
      }
    },
    {
      "address": "0x15e8c742614b5D8Db4083A41Df1A14F5D2bFB400",
      "eip2612Deposits": true,
      "isNativeUnderlying": false,
      "name": "Fluid USDtb",
      "symbol": "fUSDtb",
      "decimals": 18,
      "assetAddress": "0xC139190F447e929f090Edeb554D95AbB8b18aC1C",
      "rewards": [
        {
          "token": {
            "address": "0xa74cab633214c16808eb0b6f499c98036b227b8a",
            "name": "USDtb (wrapped)",
            "symbol": "USDtb",
            "decimals": 18,
            "price": "0",
            "chainId": "1",
            "logoUrl": null,
            "coingeckoId": null,
            "updatedAt": "2025-12-12T12:54:07.000+00:00"
          },
          "rate": "375",
          "endTime": 1766707200000,
          "type": "supply",
          "metadata": {
            "claimUrl": "https://app.merkl.xyz/opportunities/ethereum/ERC20LOGPROCESSOR/0x15e8c742614b5D8Db4083A41Df1A14F5D2bFB400BORROW_BL"
          }
        }
      ],
      "asset": {
        "address": "0xc139190f447e929f090edeb554d95abb8b18ac1c",
        "name": "USDtb",
        "symbol": "USDtb",
        "decimals": 18,
        "price": "1.001408272517",
        "chainId": "1",
        "logoUrl": "https://coin-images.coingecko.com/coins/images/52804/large/76357aa8-4ef7-446c-bad3-a3f944eeec7a.jpeg?1758277468",
        "coingeckoId": "usdtb",
        "updatedAt": "2025-12-12T12:54:05.000+00:00"
      },
      "totalAssets": "960681750787747258719168",
      "totalSupply": "947072991444502577933750",
      "convertToShares": "985834268911545717",
      "convertToAssets": "1014369282480000000",
      "rewardsRate": "0",
      "supplyRate": "90",
      "totalRate": "90",
      "liquiditySupplyData": {
        "modeWithInterest": true,
        "supply": "960681750787745222112938",
        "withdrawalLimit": "0",
        "lastUpdateTimestamp": "1765382555",
        "expandPercent": "5000",
        "expandDuration": "21600",
        "baseWithdrawalLimit": "10136112927769152107885477",
        "withdrawableUntilLimit": "960681750787745222112938",
        "withdrawable": "960681750787745222112938"
      }
    },
    {
      "address": "0x2411802D8BEA09be0aF8fD8D08314a63e706b29C",
      "eip2612Deposits": true,
      "isNativeUnderlying": false,
      "name": "Fluid Wrapped liquid staked Ether 2.0",
      "symbol": "fwstETH",
      "decimals": 18,
      "assetAddress": "0x7f39C581F595B53c5cb19bD0b3f8dA6c935E2Ca0",
      "asset": {
        "address": "0x7f39c581f595b53c5cb19bd0b3f8da6c935e2ca0",
        "name": "Wrapped liquid staked Ether 2.0",
        "symbol": "wstETH",
        "decimals": 18,
        "price": "3960.565963262161",
        "chainId": "1",
        "logoUrl": "https://coin-images.coingecko.com/coins/images/18834/large/wstETH.png?1696508295",
        "coingeckoId": "wrapped-steth",
        "updatedAt": "2025-12-12T12:54:02.000+00:00",
        "stakingApr": "254"
      },
      "totalAssets": "2704435239733090988585",
      "totalSupply": "2606010646983313705186",
      "convertToShares": "963606230497317765",
      "convertToAssets": "1037768300319000000",
      "rewardsRate": "0",
      "supplyRate": "26",
      "totalRate": "26",
      "liquiditySupplyData": {
        "modeWithInterest": true,
        "supply": "2704435239730380757203",
        "withdrawalLimit": "0",
        "lastUpdateTimestamp": "1765544147",
        "expandPercent": "5000",
        "expandDuration": "21600",
        "baseWithdrawalLimit": "3277811337614796866269",
        "withdrawableUntilLimit": "2704435239730380757203",
        "withdrawable": "2704435239730380757203"
      }
    },
    {
      "address": "0x90551c1795392094FE6D29B758EcCD233cFAa260",
      "eip2612Deposits": false,
      "isNativeUnderlying": true,
      "name": "Fluid Wrapped Ether",
      "symbol": "fWETH",
      "decimals": 18,
      "assetAddress": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
      "asset": {
        "address": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
        "name": "Wrapped Ether",
        "symbol": "WETH",
        "decimals": 18,
        "price": "3243.373127778169",
        "chainId": "1",
        "logoUrl": "https://coin-images.coingecko.com/coins/images/2518/large/weth.png?1696503332",
        "coingeckoId": "weth",
        "updatedAt": "2025-12-12T12:54:02.000+00:00"
      },
      "totalAssets": "2807890938240304507591",
      "totalSupply": "2637254472794364695628",
      "convertToShares": "939229667676167974",
      "convertToAssets": "1064702313412000000",
      "rewardsRate": "0",
      "supplyRate": "177",
      "totalRate": "177",
      "liquiditySupplyData": {
        "modeWithInterest": true,
        "supply": "2807890938239894164902",
        "withdrawalLimit": "0",
        "lastUpdateTimestamp": "1765519247",
        "expandPercent": "5000",
        "expandDuration": "21600",
        "baseWithdrawalLimit": "4036079819035619264420",
        "withdrawableUntilLimit": "2807890938239894164902",
        "withdrawable": "2807890938239894164902"
      }
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
- Tokens can have multiple reward programs active simultaneously
- The `totalAssetsInUsd` and `yearlyYieldInUsd` provide aggregate statistics across all tokens
- Some reward tokens may have a `price` of `0` or `null` values for `logoUrl` and `coingeckoId` if they are not listed on CoinGecko
- The `stakingApr` field in the asset object is only present for staking tokens (e.g., wstETH)

## Error Responses

The API may return standard HTTP error codes:

- `400 Bad Request` - Invalid parameters (e.g., invalid chain ID)
- `404 Not Found` - Chain ID or endpoint not found
- `500 Internal Server Error` - Server error

