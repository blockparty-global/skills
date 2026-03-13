# OneSourceNonce

Get the transaction count (nonce) for an address. Required for constructing and signing transactions — prevents nonce errors when batching.

## Endpoint

`GET https://skills.onesource.io/api/chain/nonce/{address}`

**Cost:** 0.003 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| address | path | yes | Wallet address (0x...) |
| block | query | no | "pending" (default) or "latest" |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/nonce/0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045?block=pending
```

### Response

```json
{
  "data": {
    "nonce": "0x42"
  },
  "meta": {
    "endpoint": "/api/chain/nonce/0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "cost_usdc": "0.003",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Get the correct nonce before signing a transaction
- Batch multiple transactions with sequential nonces (nonce, nonce+1, nonce+2...)
- Detect stuck transactions (pending nonce > latest nonce)
- Verify transaction count for an address

## Notes

- Use `block=pending` (default) to account for pending transactions in the mempool
- Use `block=latest` to get only confirmed transaction count
- Nonce is hex-encoded — parse to integer for use in transaction construction
- If `pending > latest`, there are unconfirmed transactions in the mempool

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceEstimateGas (estimate gas for the transaction)
- OneSourceLiveBalance (verify balance before sending)
- OneSourceTransactionReceipt (verify tx after sending)
