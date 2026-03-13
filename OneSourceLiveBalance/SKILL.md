# OneSourceLiveBalance

Get real-time ETH and ERC20 token balances for any wallet directly from the blockchain. Supports batch queries for multiple tokens in a single request. Fresher than indexed data — no sync delay.

## Endpoint

`GET https://skills.onesource.io/api/chain/live-balance`

**Cost:** 0.003 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| address | string | yes | Wallet address (0x...) |
| tokens | string | no | Comma-separated ERC20 contract addresses |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/live-balance?address=0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045&tokens=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48,0xdAC17F958D2ee523a2206206994597C13D831ec7
```

### Response

```json
{
  "data": {
    "address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "eth_balance": "0x1BC16D674EC80000",
    "tokens": [
      {
        "contract": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
        "balance": "0x05f5e100",
        "symbol": "USDC",
        "decimals": 6
      },
      {
        "contract": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
        "balance": "0x02faf080",
        "symbol": "USDT",
        "decimals": 6
      }
    ]
  },
  "meta": {
    "endpoint": "/api/chain/live-balance",
    "cost_usdc": "0.003",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4e5f6"
  }
}
```

## When to Use

- Check "do I have enough?" before executing a swap or transfer
- Verify token balances across multiple tokens in one call
- Need fresh data without indexing delay (balances change every block)
- Pre-flight check before signing transactions

## Notes

- ETH balance is always returned even if no tokens are specified
- Token balances include symbol and decimals decoded on-chain
- Individual token errors (e.g. invalid contract) are returned per-token without failing the whole request
- All balance values are hex-encoded wei — parse with BigInt

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceERC20Balance (indexed, historical balance snapshots)
- OneSourceEstimateGas (pair with balance check before sending)
- OneSourceSimulateCall (simulate the transaction before executing)
