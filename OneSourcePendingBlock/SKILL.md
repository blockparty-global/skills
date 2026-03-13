# OneSourcePendingBlock

Get the pending block including transactions in the mempool. Useful for MEV-aware agents, arbitrage monitoring, and liquidation detection.

## Endpoint

`GET https://skills.onesource.io/api/chain/pending`

**Cost:** 0.010 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/pending
```

### Response

```json
{
  "data": {
    "result": {
      "number": "0x1234568",
      "timestamp": "0x65a1b2c3",
      "transactions": [
        {
          "hash": "0xabc...",
          "from": "0x...",
          "to": "0x...",
          "value": "0x...",
          "input": "0x..."
        }
      ],
      "baseFeePerGas": "0x3b9aca00"
    }
  },
  "meta": {
    "endpoint": "/api/chain/pending",
    "cost_usdc": "0.010",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Monitor mempool for arbitrage opportunities
- Detect pending liquidation transactions
- MEV strategy development and monitoring
- Track pending transactions for a specific address
- Base fee prediction for next block

## Notes

- Higher cost due to compute intensity
- Availability depends on node configuration — not all nodes expose pending state
- Transaction list can change between calls (transactions get mined/dropped)
- Decode `input` field using known ABIs to identify swap/liquidation calls

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceTransactionReceipt (check if a specific tx was mined)
- OneSourceNetworkInfo (get current gas price and block number)
