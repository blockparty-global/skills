# OneSourceChainProxy

Detect proxy contracts and read their implementation address via EIP-1967 storage slots. Reads the implementation, admin, and beacon slots to identify upgradeable proxy patterns.

## Endpoint

`GET https://skills.onesource.io/api/chain/proxy/{address}`

**Cost:** 0.005 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| address | path | yes | Contract address to inspect (0x...) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/proxy/0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48
```

### Response

```json
{
  "data": {
    "address": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "is_proxy": true,
    "implementation": "0x43506849D7C04F9138D1A2050bbF3A0c054402dd",
    "admin": "0x807a96288A1A408dBC13DE2b1d087d10356395d2",
    "beacon": "",
    "is_contract": true
  },
  "meta": {
    "endpoint": "/api/chain/proxy/0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "cost_usdc": "0.005",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Determine if a contract is an upgradeable proxy
- Find the actual implementation contract behind a proxy
- Identify the admin address that controls proxy upgrades
- Debug why ERC165 `supportsInterface` returns false for known token contracts (proxy bytecode issue)
- Pair with OneSourceChainContractInfo on the implementation to get the real ERC standard support

## Notes

- Reads three EIP-1967 standardized storage slots in a single batched RPC call:
  - **Implementation**: `0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc`
  - **Admin**: `0xb53127684a568b3173ae13b9f8a6016e243e63b6e8ee1178d6a717850b5d6103`
  - **Beacon**: `0xa3f0ad74e5423aebfd80d3ef4346578335a9a72aeaee59ff6cb3582b35133d50`
- `is_proxy: true` if either the implementation or beacon slot contains a non-zero address
- Empty `implementation` + non-empty `beacon` indicates a Beacon Proxy pattern
- The admin address can upgrade the implementation — important for security analysis
- **Not all proxies use EIP-1967.** Older proxies (e.g., OpenZeppelin's unstructured storage, custom patterns) store implementation addresses elsewhere
- To get the real contract type behind a proxy, call OneSourceChainContractInfo on the `implementation` address

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceChainContractInfo (run on the implementation address for real ERC detection)
- OneSourceChainStorageRead (read arbitrary storage slots for non-EIP-1967 proxies)
- OneSourceChainContractCode (compare bytecode between proxy and implementation)
