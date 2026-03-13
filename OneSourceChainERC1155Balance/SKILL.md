# OneSourceChainERC1155Balance

Get a live ERC1155 token balance directly from the blockchain via RPC. Calls the contract's `balanceOf(address,tokenId)` function.

## Endpoint

`GET https://skills.onesource.io/api/chain/erc1155-balance`

**Cost:** 0.003 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| account | query | yes | Wallet address (0x...) |
| contract | query | yes | ERC1155 contract address (0x...) |
| token_id | query | yes | Token ID (decimal) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/erc1155-balance?account=0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045&contract=0x76BE3b62873462d2142405439777e971754E8E77&token_id=10560
```

### Response

```json
{
  "data": {
    "account": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
    "contract_address": "0x76be3b62873462d2142405439777e971754e8e77",
    "token_id": "10560",
    "balance": "1"
  },
  "meta": {
    "endpoint": "/api/chain/erc1155-balance",
    "cost_usdc": "0.003",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Check real-time ERC1155 balance with zero indexing delay
- Verify ownership before marketplace operations
- When OpenSearch is unavailable

## Notes

- Balance is a decimal string — ERC1155 supports fungible quantities (balance > 1)
- Returns "0" if the wallet does not hold this token ID
- Reverts if the contract is not ERC1155

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceERC1155Balance (indexed version with token metadata)
- OneSourceChainNFTOwner (ERC721 owner lookup via RPC)
