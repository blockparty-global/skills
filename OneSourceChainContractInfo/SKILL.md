# OneSourceChainContractInfo

Get contract information directly from the blockchain via RPC. Checks if an address is a contract, reads name/symbol, and detects ERC standard support (ERC165, ERC721, ERC1155, ERC20) via `supportsInterface`.

## Endpoint

`GET https://skills.onesource.io/api/chain/contract/{address}`

**Cost:** 0.005 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| address | path | yes | Contract or EOA address (0x...) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/contract/0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D
```

### Response

```json
{
  "data": {
    "address": "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D",
    "is_contract": true,
    "code_length": 7243,
    "name": "BoredApeYachtClub",
    "symbol": "BAYC",
    "is_erc721": true,
    "is_erc1155": false,
    "is_erc20": false,
    "is_erc165": true,
    "interfaces": ["ERC165", "ERC721"]
  },
  "meta": {
    "endpoint": "/api/chain/contract/0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D",
    "cost_usdc": "0.005",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Determine if an address is a contract or EOA
- Detect which ERC standards a contract supports
- Get contract name and symbol
- Verify contract type before making token-specific calls

## Notes

- All checks are done in a single batched RPC call (7 calls batched)
- `is_contract: false` means the address is an EOA (externally owned account) — other fields will be empty
- Interface detection uses ERC165 `supportsInterface` — proxy contracts may not respond correctly
- Name and symbol may be empty for contracts that don't implement these methods
- `code_length` is the bytecode length in bytes (half the hex length)
- For full indexed contract data (creator, creation block, ABI), use the indexed OneSourceContractInfo

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceContractInfo (indexed version with creation details, bytecode hash, enrichment data)
- OneSourceContractCode (raw bytecode via RPC)
- OneSourceChainNFTOwner (if ERC721, check token ownership)
- OneSourceChainERC20Balance (if ERC20, check balance)
