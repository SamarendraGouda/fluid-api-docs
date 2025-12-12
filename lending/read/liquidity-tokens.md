# Get Liquidity Tokens

Retrieves all available liquidity tokens with their current market data, interest rates, and utilization metrics.

## Endpoint

```
GET /1/liquidity/tokens
```

## Base URL

```
https://api.fluid.instadapp.io
```

## Path Parameters

None

## Query Parameters

None

## Example Request

```bash
curl -X GET "https://api.fluid.instadapp.io/1/liquidity/tokens"
```

## Response

Returns an array of token objects, each representing a liquidity token available on the platform with its current market state.

### Response Structure

```json
[
  {
    "address": "string",
    "name": "string",
    "symbol": "string",
    "decimals": integer,
    "price": "string",
    "chainId": "string",
    "logoUrl": "string",
    "coingeckoId": "string",
    "updatedAt": "string",
    "totalSupply": "string",
    "supplyInterestFree": "string",
    "supplyRawInterest": "string",
    "supplyRate": "string",
    "totalBorrow": "string",
    "borrowInterestFree": "string",
    "borrowRawInterest": "string",
    "borrowRate": "string",
    "revenue": "string",
    "fee": "string",
    "lastStoredUtilization": "string",
    "maxUtilization": "string",
    "interestRateModel": [ ... ]
  }
]
```

### Response Fields

#### Token Object

| Field | Type | Description |
|-------|------|-------------|
| `address` | string | Contract address of the token |
| `name` | string | Full name of the token |
| `symbol` | string | Token symbol (e.g., `ETH`, `USDC`, `WBTC`) |
| `decimals` | integer | Number of decimals for the token |
| `price` | string | Current price of the token in USD |
| `chainId` | string | Chain ID where the token exists (e.g., `"1"` for Ethereum mainnet) |
| `logoUrl` | string | URL to the token's logo image (may be `null`) |
| `coingeckoId` | string | CoinGecko identifier for the token (may be `null`) |
| `updatedAt` | string | ISO 8601 timestamp of when the data was last updated |
| `totalSupply` | string | Total supply of the token in the liquidity pool (in token decimals) |
| `supplyInterestFree` | string | Supply amount that is interest-free (in token decimals) |
| `supplyRawInterest` | string | Supply amount that accrues interest (in token decimals) |
| `supplyRate` | string | Current supply interest rate APR (basis points, e.g., `177` = 1.77%) |
| `totalBorrow` | string | Total amount borrowed against this token (in token decimals) |
| `borrowInterestFree` | string | Borrow amount that is interest-free (in token decimals) |
| `borrowRawInterest` | string | Borrow amount that accrues interest (in token decimals) |
| `borrowRate` | string | Current borrow interest rate APR (basis points, e.g., `248` = 2.48%) |
| `revenue` | string | Total revenue generated from fees (in token decimals) |
| `fee` | string | Protocol fee in basis points (e.g., `1000` = 10%) |
| `lastStoredUtilization` | string | Last stored utilization rate (basis points, e.g., `7944` = 79.44%) |
| `maxUtilization` | string | Maximum allowed utilization rate (basis points, e.g., `10000` = 100%) |
| `interestRateModel` | array | Array of interest rate model configurations (see [Interest Rate Model](#interest-rate-model)) |

#### Interest Rate Model

The `interestRateModel` array contains objects that define how interest rates change based on utilization. Each object represents either a supply or borrow rate model.

| Field | Type | Description |
|-------|------|-------------|
| `type` | string | Type of rate model (`"supply"` or `"borrow"`) |
| `rate` | string | Current rate for this model type (basis points) |
| `data` | array | Array of utilization rate breakpoints (see [Rate Data Point](#rate-data-point)) |

#### Rate Data Point

Each data point in the interest rate model defines a utilization threshold and the corresponding interest rate.

| Field | Type | Description |
|-------|------|-------------|
| `utilization` | string | Utilization threshold in basis points (e.g., `8800` = 88%) |
| `rate` | string | Interest rate at this utilization level (basis points, e.g., `247` = 2.47%) |

## Example Response

```json
[
  {
    "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
    "name": "ETH",
    "symbol": "ETH",
    "decimals": 18,
    "price": "3248.004616887584",
    "chainId": "1",
    "logoUrl": "https://coin-images.coingecko.com/coins/images/279/large/ethereum.png?1696501628",
    "coingeckoId": "ethereum",
    "updatedAt": "2025-12-12T12:57:01.000+00:00",
    "totalSupply": "86785086220868066917708",
    "supplyInterestFree": "0",
    "supplyRawInterest": "81511126590569477832704",
    "supplyRate": "177",
    "totalBorrow": "68942111836419987376346",
    "borrowInterestFree": "0",
    "borrowRawInterest": "63545121014813751771136",
    "borrowRate": "248",
    "revenue": "6271611363194295204",
    "fee": "1000",
    "lastStoredUtilization": "7944",
    "maxUtilization": "10000",
    "interestRateModel": [
      {
        "type": "supply",
        "rate": "177",
        "data": [
          {
            "utilization": "0",
            "rate": "0"
          },
          {
            "utilization": "8800",
            "rate": "247"
          },
          {
            "utilization": "9300",
            "rate": "360"
          },
          {
            "utilization": "10000",
            "rate": "9000"
          }
        ]
      },
      {
        "type": "borrow",
        "rate": "248",
        "data": [
          {
            "utilization": "0",
            "rate": "0"
          },
          {
            "utilization": "8800",
            "rate": "275"
          },
          {
            "utilization": "9300",
            "rate": "400"
          },
          {
            "utilization": "10000",
            "rate": "10000"
          }
        ]
      }
    ]
  },
  {
    "address": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
    "name": "USD Coin",
    "symbol": "USDC",
    "decimals": 6,
    "price": "0.999879023901",
    "chainId": "1",
    "logoUrl": "https://coin-images.coingecko.com/coins/images/6319/large/usdc.png?1696506694",
    "coingeckoId": "usd-coin",
    "updatedAt": "2025-12-12T12:57:01.000+00:00",
    "totalSupply": "297978288214614",
    "supplyInterestFree": "0",
    "supplyRawInterest": "264500807924991",
    "supplyRate": "390",
    "totalBorrow": "228992731055874",
    "borrowInterestFree": "0",
    "borrowRawInterest": "195164690759652",
    "borrowRate": "565",
    "revenue": "38566802813",
    "fee": "1000",
    "lastStoredUtilization": "7688",
    "maxUtilization": "10000",
    "interestRateModel": [
      {
        "type": "supply",
        "rate": "390",
        "data": [
          {
            "utilization": "0",
            "rate": "0"
          },
          {
            "utilization": "8500",
            "rate": "562"
          },
          {
            "utilization": "9300",
            "rate": "810"
          },
          {
            "utilization": "10000",
            "rate": "3600"
          }
        ]
      },
      {
        "type": "borrow",
        "rate": "565",
        "data": [
          {
            "utilization": "0",
            "rate": "0"
          },
          {
            "utilization": "8500",
            "rate": "625"
          },
          {
            "utilization": "9300",
            "rate": "900"
          },
          {
            "utilization": "10000",
            "rate": "4000"
          }
        ]
      }
    ]
  }
]
```

## Notes

- All numeric values are returned as strings to avoid precision loss with large numbers
- Rates are provided in basis points (1 basis point = 0.01%)
  - Example: `177` basis points = 1.77% APR
- Utilization rates are also in basis points
  - Example: `7944` basis points = 79.44% utilization
- The `interestRateModel` data points define a piecewise linear interest rate curve
  - Rates are interpolated between breakpoints based on current utilization
- The `fee` field represents the protocol fee in basis points (e.g., `1000` = 10%)
- `logoUrl` and `coingeckoId` may be `null` for tokens that don't have CoinGecko listings
- The `price` field may be `"0"` for tokens without a price feed
- `supplyInterestFree` and `borrowInterestFree` represent amounts that don't accrue interest
- `supplyRawInterest` and `borrowRawInterest` represent amounts that do accrue interest
- The sum of interest-free and raw interest amounts equals the total supply/borrow

## Error Responses

The API may return standard HTTP error codes:

- `400 Bad Request` - Invalid request parameters
- `404 Not Found` - Endpoint not found
- `500 Internal Server Error` - Server error

