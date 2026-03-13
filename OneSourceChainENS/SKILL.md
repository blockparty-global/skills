# OneSourceChainENS

Resolve ENS (Ethereum Name Service) names via RPC. Supports forward resolution (name to address) and reverse resolution (address to name). Auto-detects direction based on input format.

## Endpoint

`GET https://skills.onesource.io/api/chain/ens/{input}`

**Cost:** 0.005 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| input | path | yes | ENS name (e.g., `vitalik.eth`) or address (0x...) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Examples

### Forward Resolution (Name → Address)

```
GET /api/chain/ens/vitalik.eth
```

```json
{
  "data": {
    "name": "vitalik.eth",
    "address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "resolver": "0x4976fb03C32e5B8cfe2b6cCB31c09Ba78EBaBa41",
    "is_forward": true,
    "is_reverse": false
  },
  "meta": {
    "endpoint": "/api/chain/ens/vitalik.eth",
    "cost_usdc": "0.005",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

### Reverse Resolution (Address → Name)

```
GET /api/chain/ens/0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
```

```json
{
  "data": {
    "name": "vitalik.eth",
    "address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "resolver": "0xa58E81fe9b61B5c3fE2AFD33CF304c454AbFc7Cb",
    "is_forward": false,
    "is_reverse": true
  },
  "meta": {
    "endpoint": "/api/chain/ens/0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "cost_usdc": "0.005",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Convert human-readable ENS names to addresses before making contract calls
- Display a human-readable name for a wallet address
- Verify ENS ownership or resolver configuration
- Build agent-friendly wallet identification

## Notes

- Auto-detects direction: 42-char `0x` input triggers reverse lookup, otherwise forward
- Uses the ENS registry at `0x00000000000C2E074eC69A0dFb2997BA6C7d2e1e` (same on mainnet, goerli, sepolia)
- Forward: calls `resolver(bytes32)` then `addr(bytes32)` on the resolver contract
- Reverse: computes `<addr>.addr.reverse` namehash, then calls `name(bytes32)` on the reverse resolver
- Returns `error` field if no resolver is set or no record exists (not an HTTP error)
- ENS names only work on chains with the ENS registry deployed (Ethereum mainnet and Sepolia). Use `?network=ethereum` (default) for ENS
- Subdomains are supported (e.g., `sub.vitalik.eth`)

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceChainContractInfo (check if the resolved address is a contract)
- OneSourceChainLiveBalance (check balance of a resolved address)
