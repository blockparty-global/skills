# OneSourceStorageRead

Read a raw storage slot from a contract. Access any on-chain state including private variables, mapping values, and custom data structures.

## Endpoint

`GET https://skills.onesource.io/api/chain/storage`

**Cost:** 0.005 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| contract | query | yes | Contract address (0x...) |
| slot | query | yes | Storage slot position (hex, 0x...) |
| block | query | no | Block number or "latest" (default: "latest") |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/storage?contract=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&slot=0x0
```

### Response

```json
{
  "data": {
    "value": "0x000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
  },
  "meta": {
    "endpoint": "/api/chain/storage",
    "cost_usdc": "0.005",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Read private variables not exposed via public getters
- Access mapping values by computing the storage slot (keccak256)
- Inspect proxy implementation addresses (slot 0x360894...)
- Read protocol-specific state (e.g., Uniswap pool reserves at known slots)
- Historical state reads at specific blocks

## Notes

- Storage slots are 32 bytes (256 bits)
- Mapping slot = keccak256(abi.encode(key, mappingSlot))
- Array elements: keccak256(arraySlot) + index
- Use Solidity storage layout documentation for the target contract
- Proxy implementation slot: `0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc`

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceContractCode (verify the contract exists first)
- OneSourceContractInfo (get ABI for understanding storage layout)
- OneSourceSimulateCall (prefer view functions over raw storage when available)
