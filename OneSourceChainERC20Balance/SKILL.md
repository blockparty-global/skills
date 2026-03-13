# OneSourceChainERC20Balance

Get a live ERC20 token balance directly from the blockchain via RPC. Returns balance, name, symbol, and decimals in a single batched call. No indexing delay.

## Endpoint

`GET https://skills.onesource.io/api/chain/erc20-balance`

**Cost:** 0.003 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| account | query | yes | Wallet address (0x...) |
| token | query | yes | ERC20 token contract address (0x...) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/erc20-balance?account=0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045&token=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48
```

### Response

```json
{
  "data": {
    "account": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
    "contract_address": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
    "balance": "100000000",
    "symbol": "USDC",
    "name": "USD Coin",
    "decimals": 6
  },
  "meta": {
    "endpoint": "/api/chain/erc20-balance",
    "cost_usdc": "0.003",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Need real-time balance with zero indexing delay
- Pre-transaction balance checks
- Verifying balances before executing swaps or transfers
- When OpenSearch is unavailable

## Notes

- Balance is a decimal string in raw token units — divide by 10^decimals for human-readable
- Uses a single batched RPC call (balanceOf + name + symbol + decimals)
- Returns "0" balance if the wallet holds no tokens (not an error)
- Name/symbol/decimals may be empty for non-standard tokens

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceERC20Balance (indexed version — cheaper, includes updated_at timestamp)
- OneSourceLiveBalance (ETH + multiple ERC20 balances in one call)
- OneSourceChainERC20Transfers (transfer history via RPC)
