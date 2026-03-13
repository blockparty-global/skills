# OneSourceChainTotalSupply

Get the total supply of an ERC20 token or ERC721 NFT collection directly from the blockchain via RPC. Returns supply along with token metadata and type detection.

## Endpoint

`GET https://skills.onesource.io/api/chain/total-supply`

**Cost:** 0.003 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| contract | query | yes | Token contract address (0x...) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Examples

### ERC20 Token

```
GET /api/chain/total-supply?contract=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48
```

```json
{
  "data": {
    "contract_address": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "total_supply": "26000000000000",
    "name": "USD Coin",
    "symbol": "USDC",
    "decimals": 6,
    "is_erc721": false
  },
  "meta": {
    "endpoint": "/api/chain/total-supply",
    "cost_usdc": "0.003",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

### ERC721 Collection

```
GET /api/chain/total-supply?contract=0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D
```

```json
{
  "data": {
    "contract_address": "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D",
    "total_supply": "10000",
    "name": "BoredApeYachtClub",
    "symbol": "BAYC",
    "decimals": 0,
    "is_erc721": true
  },
  "meta": {
    "endpoint": "/api/chain/total-supply",
    "cost_usdc": "0.003",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Get the circulating supply of an ERC20 token
- Get the collection size of an ERC721 NFT project
- Calculate market cap (total supply × price)
- Determine rarity ratios for NFT collections
- Verify token issuance hasn't changed

## Notes

- Calls `totalSupply()` in a batch with `name()`, `symbol()`, `decimals()`, and `supportsInterface(ERC721)`
- For ERC20: divide `total_supply` by `10^decimals` for human-readable amount (e.g., `26000000000000` / `10^6` = 26,000,000 USDC)
- For ERC721: `total_supply` is the number of NFTs minted. `decimals` will be 0
- `is_erc721: true` indicates the contract supports the ERC721 interface (via ERC165)
- Some contracts don't implement `totalSupply()` — the call will fail with an error

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceChainContractInfo (full ERC standard detection)
- OneSourceChainERC20Balance (individual wallet balance)
- OneSourceChainERC721Tokens (enumerate owned tokens in a collection)
