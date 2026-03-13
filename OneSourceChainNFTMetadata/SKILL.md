# OneSourceChainNFTMetadata

Get NFT metadata directly from the blockchain via RPC. Calls `tokenURI` (ERC721) or `uri` (ERC1155), then fetches and parses the metadata JSON. Resolves IPFS, Arweave, and data URIs automatically.

## Endpoint

`GET https://skills.onesource.io/api/chain/nft-metadata`

**Cost:** 0.008 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| contract | query | yes | NFT contract address (0x...) |
| token_id | query | yes | Token ID (decimal) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/nft-metadata?contract=0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D&token_id=1234
```

### Response

```json
{
  "data": {
    "contract_address": "0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d",
    "token_id": "1234",
    "token_uri": "ipfs://QmeSjSinHpPnmXmspMjwiXyN6zS4E9zccariGR3jxcaWtq/1234",
    "protocol": "ipfs",
    "metadata": {
      "name": "Bored Ape #1234",
      "description": "A unique Bored Ape from the BAYC collection",
      "image": "https://ipfs.io/ipfs/QmRRPWG96cmgTn2qSzjwr2qvfNEuhunv6FNeMFGa9bx6mQ",
      "attributes": [
        {"trait_type": "Background", "value": "Orange"},
        {"trait_type": "Fur", "value": "Dark Brown"}
      ]
    }
  },
  "meta": {
    "endpoint": "/api/chain/nft-metadata",
    "cost_usdc": "0.008",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Get fresh NFT metadata directly from the source
- When the indexed metadata is missing or stale
- Verify metadata hasn't changed since indexing
- Display NFT details (name, image, traits)

## Notes

- Tries ERC721 `tokenURI` first, falls back to ERC1155 `uri`
- Automatically resolves all URI formats:
  - `ipfs://QmHash`, `/ipfs/QmHash`, bare CIDs (`Qm...`, `bafy...`)
  - IPFS gateway URLs (`https://ipfs.io/ipfs/...`, etc.)
  - `ar://txid` (Arweave)
  - `data:application/json;base64,...` and `data:application/json,...`
  - ERC1155 `{id}` template substitution
- Nested IPFS/Arweave URIs in `image`, `animation_url`, `external_url` are also resolved
- If the URI points to an image (not JSON), returns synthetic metadata with `{"image": "<url>"}`
- `protocol` field indicates the detected URI type: `ipfs`, `arweave`, `data`, `http`
- `fetch_error` is set if the metadata could not be fetched (timeout, 404, etc.)
- Configure the IPFS gateway via the `IPFS_GATEWAY` environment variable
- Higher cost than indexed version due to RPC call + external HTTP fetch

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceNFTMetadata (indexed version — faster, cheaper, pre-cached)
- OneSourceChainNFTOwner (who owns this NFT via RPC)
- OneSourceNFTMedia (processed media URLs from index)
