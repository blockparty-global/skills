# OneSourceNetworkInfo

Get chain ID, network version, current block number, and gas price in a single batched call. Confirms you're on the right chain and provides essential network state.

## Endpoint

`GET https://skills.onesource.io/api/chain/network-info`

**Cost:** 0.001 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/network-info
```

### Response

```json
{
  "data": {
    "network": "ethereum",
    "chain_id": "0x1",
    "net_version": "1",
    "block_number": "0x12a05f2",
    "gas_price": "0x3b9aca00"
  },
  "meta": {
    "endpoint": "/api/chain/network-info",
    "cost_usdc": "0.001",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Confirm you're connected to the correct chain before any operation
- Get current gas price for fee estimation
- Get latest block number for block-based queries
- Health check the RPC connection

## Notes

- All values are hex-encoded
- Chain ID 1 = Ethereum mainnet, 8453 = Base, 137 = Polygon
- Gas price is in wei — divide by 1e9 for gwei
- This batches 4 RPC calls into one request for efficiency
- Cheapest endpoint — good for periodic polling

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceEstimateGas (more accurate gas for a specific transaction)
- OneSourceBlockDetails (get full block data)
