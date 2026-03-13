# OneSourceEstimateGas

Estimate gas required for a transaction before submitting. Prevents failed transactions and helps set accurate gas limits.

## Endpoint

`POST https://skills.onesource.io/api/chain/estimate-gas`

**Cost:** 0.004 USDC (Base)

## Parameters (JSON body)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| to | string | yes | Target contract or recipient address |
| data | string | no | ABI-encoded call data (hex) |
| from | string | no | Sender address |
| value | string | no | ETH value in wei (hex) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
POST /api/chain/estimate-gas
Content-Type: application/json

{
  "to": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
  "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa960450000000000000000000000000000000000000000000000000000000005f5e100",
  "from": "0x1234567890abcdef1234567890abcdef12345678"
}
```

### Response

```json
{
  "data": {
    "gas": "0xc350"
  },
  "meta": {
    "endpoint": "/api/chain/estimate-gas",
    "cost_usdc": "0.004",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Before signing and submitting any transaction
- Calculate gas cost in ETH (gas * gas_price)
- Set gas limit with a safety margin (estimate * 1.2)
- Detect transactions that would revert (estimate fails)

## Notes

- If estimation fails, the transaction would likely revert on-chain
- Gas estimate is for current block state — may change by execution time
- Add 20% buffer to the estimate for safety
- Pair with OneSourceNetworkInfo to get current gas price

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceSimulateCall (simulate the call first to check output)
- OneSourceLiveBalance (verify ETH balance covers gas + value)
- OneSourceNonce (get correct nonce for the transaction)
- OneSourceNetworkInfo (get current gas price)
