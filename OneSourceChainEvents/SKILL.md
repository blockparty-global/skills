# OneSourceChainEvents

Query event logs directly from the blockchain via `eth_getLogs`. Filter by contract address and/or event topic within a block range.

## Endpoint

`GET https://skills.onesource.io/api/chain/events`

**Cost:** 0.005 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| contract | query | no | Contract address to filter by (0x...) |
| topic | query | no | Event topic0 hash to filter by (0x..., 32-byte keccak) |
| from_block | query | no | Start block in hex (e.g., 0x12A05F0). Defaults to "latest" |
| to_block | query | no | End block in hex (e.g., 0x12A1000). Defaults to "latest" |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/events?contract=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&topic=0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef&from_block=0x1312D00&to_block=0x1312D64
```

### Response

```json
{
  "data": {
    "logs": [
      {
        "address": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
        "topics": [
          "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
          "0x000000000000000000000000sender...",
          "0x000000000000000000000000receiver..."
        ],
        "data": "0x0000000000000000000000000000000000000000000000000000000005f5e100",
        "blockNumber": "0x1312D05",
        "transactionHash": "0xabc...",
        "logIndex": "0x2"
      }
    ],
    "count": 1
  },
  "meta": {
    "endpoint": "/api/chain/events",
    "cost_usdc": "0.005",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## Common Event Topics

| Event | Topic0 |
|-------|--------|
| Transfer(address,address,uint256) | `0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef` |
| Approval(address,address,uint256) | `0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925` |
| Swap (Uniswap V2) | `0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822` |

## When to Use

- Query raw event logs from any contract
- Monitor specific events within a block range
- Trace ERC20 transfers, approvals, swaps, etc.
- When you need the freshest data with no indexing delay

## Notes

- Block numbers must be in hex format (e.g., `0x12A05F0` = 19,530,224)
- **Large block ranges are slower.** The larger the range, the more data the RPC node must scan
- **Maximum block range: 100,000 blocks.** Requests exceeding this will be rejected by the RPC node
- If neither `from_block` nor `to_block` is specified, defaults to "latest" (single block)
- At least one of `contract` or `topic` is strongly recommended to avoid massive result sets
- Returns raw hex values — topics and data are ABI-encoded
- For decoded, human-readable events, use the indexed OneSourceEvents endpoint

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceEvents (indexed version with decoded event names and smaller response size)
- OneSourceChainERC20Transfers (pre-filtered for ERC20 Transfer events via RPC)
- OneSourceChainTransactionDetails (get all logs for a specific transaction)
