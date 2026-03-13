# OneSourceContractCode

Check if an address is a smart contract or an EOA (externally owned account), and retrieve the contract bytecode. Essential safety check before interacting with an address.

## Endpoint

`GET https://skills.onesource.io/api/chain/code/{address}`

**Cost:** 0.003 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| address | path | yes | Address to check (0x...) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/code/0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48
```

### Response

```json
{
  "data": {
    "address": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "is_contract": true,
    "code_length": 5765,
    "code": "0x6080604052..."
  },
  "meta": {
    "endpoint": "/api/chain/code/0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "cost_usdc": "0.003",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Verify an address is a contract before calling it
- Safety check: ensure you're not sending tokens to a contract that can't handle them
- Detect proxy contracts (code is minimal, delegates to implementation)
- Verify a contract is deployed before interacting

## Notes

- `is_contract: false` means the address is an EOA (regular wallet)
- `code: "0x"` with `is_contract: false` for EOAs
- For richer contract info (name, ABI, verification), use OneSourceContractInfo

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceContractInfo (indexed contract metadata, ABI, verification status)
- OneSourceSimulateCall (call a function on the contract)
- OneSourceStorageRead (read raw storage slots)
