# OneSourceChainNFTOwner

Get the current owner of an ERC721 NFT directly from the blockchain via RPC. Calls `ownerOf(tokenId)` on the contract.

## Endpoint

`GET https://skills.onesource.io/api/chain/nft-owner`

**Cost:** 0.003 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| contract | query | yes | NFT contract address (0x...) |
| token_id | query | yes | Token ID (decimal) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/nft-owner?contract=0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D&token_id=1234
```

### Response

```json
{
  "data": {
    "contract_address": "0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d",
    "token_id": "1234",
    "owner": "0x9a8f92a830a5cb89a3816e3d267cb7791c16b04d"
  },
  "meta": {
    "endpoint": "/api/chain/nft-owner",
    "cost_usdc": "0.003",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Verify current NFT ownership in real-time
- Pre-transfer ownership checks
- When the indexed owner data is stale or unavailable
- ERC721 only — for ERC1155, use OneSourceChainERC1155Balance

## Notes

- Returns the current on-chain owner address
- Reverts with an error if the token ID does not exist or the contract is not ERC721
- For ERC1155 tokens (multi-holder), use the balance endpoint instead
- This is a live RPC call with no caching — always returns the latest state

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceNFTOwner (indexed version — may have slight sync delay)
- OneSourceChainNFTMetadata (get metadata for this token via RPC)
- OneSourceChainERC1155Balance (check ERC1155 balance via RPC)
