# OneSourceSimulateCall

Simulate a contract call (eth_call) without sending a transaction. Returns the call result or decoded revert reason. Essential for swap simulations, yield checks, approval verification, and revert prediction.

## Endpoint

`POST https://skills.onesource.io/api/chain/call`

**Cost:** 0.005 USDC (Base)

## Parameters (JSON body)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| to | string | yes | Contract address |
| data | string | yes | ABI-encoded call data (hex) |
| from | string | no | Sender address (affects msg.sender in the call) |
| value | string | no | ETH value in wei (hex) |
| block | string | no | Block number or "latest"/"pending" (default: "latest") |
| network | string | no | Blockchain network: `ethereum` (default), `sepolia`, `avax` |

## Example

### Request

```
POST /api/chain/call
Content-Type: application/json

{
  "to": "0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D",
  "data": "0xd06ca61f0000000000000000000000000000000000000000000000000de0b6b3a764000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000002000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc2000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
  "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
}
```

### Response (success)

```json
{
  "data": {
    "result": "0x0000000000000000000000000000000000000000000000000000000005f5e100"
  },
  "meta": {
    "endpoint": "/api/chain/call",
    "cost_usdc": "0.005",
    "payment_chain": "base",
    "payment_token": "USDC",
    "request_id": "a1b2c3d4"
  }
}
```

### Response (revert)

```json
{
  "data": {
    "error": "RPC error -32000: execution reverted",
    "revert_reason": "Insufficient liquidity"
  },
  "meta": { "..." }
}
```

## When to Use

- Simulate a swap to get expected output amounts
- Check token allowances (call allowance() view function)
- Predict if a transaction will revert before spending gas
- Read any view/pure function on any contract
- Check DeFi protocol state (TVL, rates, positions)

## Notes

- This does NOT submit a transaction or cost gas on-chain
- The `from` field affects `msg.sender` in the simulation
- Use `block: "pending"` to simulate against pending state
- Decode the hex result using the contract's ABI
- If the call reverts, the revert reason is decoded when available

## Supported Networks

All endpoints accept an optional `?network=` parameter. Available networks:

| Network | Description |
|---------|-------------|
| `ethereum` | Ethereum mainnet (default) |
| `sepolia` | Ethereum Sepolia testnet |
| `avax` | Avalanche C-Chain |

Omit the parameter to use the default network (ethereum).

## Related Skills

- OneSourceEstimateGas (estimate gas cost for the same call)
- OneSourceLiveBalance (check balance before calling)
- OneSourceContractCode (verify target is a contract)
