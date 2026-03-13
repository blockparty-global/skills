# OneSourceChainTransactionDetails

Get full transaction details and receipt directly from the blockchain via RPC. Returns both the transaction object and the receipt (status, gas used, logs) in a single batched call.

## Endpoint

`GET https://skills.onesource.io/api/chain/tx/{hash}`

**Cost:** 0.008 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| hash | path | yes | Transaction hash (0x...) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/tx/0xabc123def456...
```

### Response

```json
{
  "data": {
    "transaction": {
      "hash": "0xabc123...",
      "from": "0xsender...",
      "to": "0xreceiver...",
      "value": "0x0",
      "gas": "0x5208",
      "gasPrice": "0x3b9aca00",
      "input": "0x...",
      "blockNumber": "0x12a05f0",
      "blockHash": "0x...",
      "nonce": "0x5"
    },
    "receipt": {
      "status": "0x1",
      "gasUsed": "0x5208",
      "logs": [],
      "contractAddress": null,
      "blockNumber": "0x12a05f0",
      "transactionIndex": "0x0"
    }
  },
  "meta": {
    "endpoint": "/api/chain/tx/0xabc123...",
    "cost_usdc": "0.008",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Get full transaction details including receipt and logs
- Verify transaction status (success/failure)
- Check gas usage for a specific transaction
- When you need raw RPC data without indexing transformations

## Notes

- Returns raw RPC hex values (block numbers, gas, values)
- `status: "0x1"` = success, `status: "0x0"` = reverted
- `receipt.logs` contains raw event logs — use OneSourceChainEvents for decoded logs
- Transaction and receipt are fetched in a single batched RPC call
- Returns null fields if the transaction hash is not found

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceTransactionDetails (indexed version with decoded events and human-readable values)
- OneSourceTransactionReceipt (receipt only via RPC)
- OneSourceChainEvents (query event logs by contract/topic)
