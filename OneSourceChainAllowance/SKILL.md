# OneSourceChainAllowance

Check ERC20 token allowance (approved spending amount) directly from the blockchain via RPC. Returns the amount a spender is approved to transfer on behalf of the owner, along with token metadata.

## Endpoint

`GET https://skills.onesource.io/api/chain/allowance`

**Cost:** 0.003 USDC (Base)

## Parameters

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| token | query | yes | ERC20 token contract address (0x...) |
| owner | query | yes | Token owner address (0x...) |
| spender | query | yes | Approved spender address (0x...) |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
GET /api/chain/allowance?token=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&owner=0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045&spender=0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45
```

### Response

```json
{
  "data": {
    "owner": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "spender": "0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45",
    "token": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    "allowance": "115792089237316195423570985008687907853269984665640564039457584007913129639935",
    "symbol": "USDC",
    "name": "USD Coin",
    "decimals": 6
  },
  "meta": {
    "endpoint": "/api/chain/allowance",
    "cost_usdc": "0.003",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

## When to Use

- Check if a swap router or DEX has been approved to spend tokens
- Verify approval amount before executing a trade
- Audit which contracts have spending permissions on a wallet
- Pre-flight check: "can this spender transfer X tokens from this owner?"

## Notes

- Calls `allowance(address,address)` in a batch with `symbol()`, `name()`, and `decimals()`
- Allowance is returned as a raw uint256 decimal string — divide by `10^decimals` for human-readable amount
- Max uint256 (`115792089...935`) means unlimited approval
- An allowance of `0` means no approval or previously revoked
- Token metadata (symbol, name, decimals) may be empty for non-standard contracts

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceChainERC20Balance (check actual balance after verifying allowance)
- OneSourceChainERC20Transfers (track Transfer events after approval is used)
- OneSourceChainEvents (query Approval events with topic `0x8c5be1e5...`)
