# OneSourceChainERC20Transfers

Query ERC20 Transfer event logs directly from the blockchain via `eth_getLogs`. Filter by token contract and/or wallet address within a block range.

## Endpoint

`GET https://skills.onesource.io/api/chain/erc20-transfers`

**Cost:** 0.005 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| token | query | no | ERC20 token contract address (0x...) |
| wallet | query | no | Wallet address — returns transfers sent from or received by this address |
| from_block | query | no | Start block in hex (e.g., 0x12A05F0). Defaults to "latest" |
| to_block | query | no | End block in hex (e.g., 0x12A1000). Defaults to "latest" |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/erc20-transfers?token=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&wallet=0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045&from_block=0x1312D00&to_block=0x1312D64
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
          "0x000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045",
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
    "endpoint": "/api/chain/erc20-transfers",
    "cost_usdc": "0.005",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Track ERC20 token transfers in a block range
- Find all transfers for a specific wallet (sent or received)
- Monitor token activity for a specific contract
- When you need the freshest transfer data with no indexing delay

## Notes

- Uses the Transfer(address,address,uint256) event topic: `0xddf252ad...`
- Block numbers must be in hex format (e.g., `0x12A05F0` = 19,530,224)
- **Large block ranges are slower.** The larger the range, the more data the RPC node must scan
- **Maximum block range: 100,000 blocks.** Requests exceeding this will be rejected by the RPC node
- If neither `from_block` nor `to_block` is specified, defaults to "latest" (single block)
- When `wallet` is specified, two queries are made (sender + receiver) and results are merged
- Returns raw hex values — `data` contains the transfer amount (uint256), `topics[1]` is sender, `topics[2]` is receiver
- At least one of `token` or `wallet` is recommended to avoid massive result sets
- Also catches ERC721 Transfer events (same signature) — check `data` length to distinguish

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceERC20Transfers (indexed version with decoded values and token metadata)
- OneSourceChainEvents (generic event log query for any event type)
- OneSourceChainERC20Balance (check current balance via RPC)
