# OneSourceTransactionReceipt

Get the receipt for a mined transaction — status, gas used, logs, and contract address (if deployment). Used for post-execution verification after submitting a transaction.

## Endpoint

`GET https://skills.onesource.io/api/chain/receipt/{hash}`

**Cost:** 0.005 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| hash | path | yes | Transaction hash (0x...) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/receipt/0x5c504ed432cb51138bcf09aa5e8a410dd4a1e204ef84bfed1be16dfba1b22060
```

### Response

```json
{
  "data": {
    "result": {
      "transactionHash": "0x5c504ed...",
      "blockNumber": "0x12345",
      "status": "0x1",
      "gasUsed": "0x5208",
      "cumulativeGasUsed": "0xa410",
      "logs": [],
      "contractAddress": null
    }
  },
  "meta": {
    "endpoint": "/api/chain/receipt/0x5c504ed...",
    "cost_usdc": "0.005",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Verify a submitted transaction succeeded (status = 0x1)
- Get actual gas used for cost accounting
- Read emitted event logs from the transaction
- Get the deployed contract address from a creation tx
- Poll in a loop after sending a transaction until receipt appears

## Notes

- Returns null if the transaction is still pending (not yet mined)
- `status`: `0x1` = success, `0x0` = reverted
- `logs` contains raw event logs — decode using contract ABI
- For richer decoded data, use OneSourceTransactionDetails (indexed)

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceTransactionDetails (indexed version with decoded events)
- OneSourceNonce (check nonce before submitting next tx)
- OneSourceSimulateCall (simulate before submitting)
