# OneSourceChainERC721Tokens

Enumerate ERC721 tokens owned by an address directly from the blockchain via RPC. Uses the ERC721Enumerable extension to list specific token IDs owned by a wallet.

## Endpoint

`GET https://skills.onesource.io/api/chain/erc721-tokens`

**Cost:** 0.008 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| contract | query | yes | ERC721 contract address (0x...) |
| owner | query | yes | Wallet address to enumerate tokens for (0x...) |
| max | query | no | Maximum tokens to return (default 50, max 100) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/erc721-tokens?contract=0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D&owner=0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045&max=10
```

### Response

```json
{
  "data": {
    "contract_address": "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D",
    "owner": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "balance": "3",
    "token_ids": ["1234", "5678", "9012"],
    "is_enumerable": true
  },
  "meta": {
    "endpoint": "/api/chain/erc721-tokens",
    "cost_usdc": "0.008",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- List all NFTs from a specific collection that a wallet owns
- Get specific token IDs to then fetch metadata for each
- Verify which tokens a wallet holds before making transfer calls
- Build a portfolio view of NFTs for a specific collection

## Notes

- First checks `balanceOf(address)` and `supportsInterface(ERC721Enumerable)` in a batch
- If the contract **does not** support ERC721Enumerable, returns `is_enumerable: false` with an error — use indexed OneSourceWalletNFTs instead
- Then batch-calls `tokenOfOwnerByIndex(address, index)` for each index up to `balance` or `max`
- **Higher cost** due to potentially many RPC calls (1 per token ID, batched)
- Default `max` is 50, hard cap is 100 to prevent excessive RPC usage
- If a wallet owns more tokens than `max`, only the first `max` token IDs are returned — check `balance` to see the total
- Not all ERC721 contracts implement Enumerable — many popular collections (e.g., ERC721A) do not
- For non-enumerable contracts, use OneSourceWalletNFTs (indexed data) or OneSourceChainEvents to find Transfer events

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceChainNFTMetadata (fetch metadata for each token ID returned)
- OneSourceChainNFTOwner (verify ownership of a specific token)
- OneSourceWalletNFTs (indexed version — works for all ERC721, not just enumerable)
- OneSourceChainTotalSupply (get collection size)
