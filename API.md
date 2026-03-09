# API Reference

Public REST API and WebSocket feed for Blume Finance on-chain data.

**Base URL:** `https://api.blumefi.com`

No authentication required. Rate limit: 100 requests per minute per IP.

Data is indexed from on-chain events and updated every ~5 seconds.

---

## Health

### `GET /health`

Returns indexer status and uptime.

---

## Blumepad (Token Launchpad)

### `GET /pad/tokens`

List all Blumepad tokens.

**Query parameters:**

| Param | Type | Description |
|-------|------|-------------|
| `chain` | `mainnet` \| `testnet` | Filter by network |
| `filter` | `graduated` \| `active` | Token status |
| `sort` | `marketcap` \| `price` \| `newest` \| `last_active` | Sort order (default: `newest`) |
| `search` | string | Name or symbol substring match |
| `creator` | address | Filter by creator wallet |
| `limit` | number | Max 100 (default: 50) |
| `offset` | number | Pagination offset |

**Response:**

```json
{
  "tokens": [
    {
      "address": "0x...",
      "name": "Token Name",
      "symbol": "TKN",
      "description": "...",
      "imageURI": "https://arweave.net/...",
      "creator": "0x...",
      "graduated": true,
      "pairAddress": "0x...",
      "price": "0.00123",
      "marketCap": "1234567.89",
      "volume24h": "456.78",
      "tradeCount24h": 12,
      "totalTradeCount": 345,
      "uniqueTraders": 89,
      "totalSupply": "1000000000000000000000000000",
      "devAllocationBps": 0,
      "progress": 100,
      "graduationReserve": "500000000000000000000",
      "createdAt": "2026-02-15T12:00:00.000Z",
      "createdViaPlatform": true,
      "chainId": 1440000
    }
  ],
  "total": 42,
  "nextOffset": 50
}
```

`price` and `marketCap` are in XRP. `totalSupply` and `graduationReserve` are raw wei strings (18 decimals).

---

### `GET /pad/tokens/:address`

Single token detail. Same fields as the list endpoint, plus `graduatedAt`, `blockNumber`, and `creatorTokenCount`.

---

### `GET /pad/tokens/:address/trades`

Trade history for a token, merged from bonding curve and DEX activity, sorted by time (newest first).

**Query parameters:**

| Param | Type | Description |
|-------|------|-------------|
| `limit` | number | Max 100 |
| `cursor` | string | Pagination cursor |
| `type` | `buy` \| `sell` | Filter by trade direction |

**Response:**

```json
{
  "trades": [
    {
      "id": "...",
      "type": "buy",
      "source": "dex",
      "trader": "0x...",
      "xrpAmount": "10000000000000000000",
      "tokenAmount": "8130000000000000000000",
      "pricePerToken": "0.00123",
      "txHash": "0x...",
      "blockNumber": 4850000,
      "timestamp": "2026-03-01T08:30:00.000Z"
    }
  ],
  "nextCursor": "..."
}
```

`source` is `"curve"` for bonding curve trades (pre-graduation) and `"dex"` for BlumeSwap trades (post-graduation). Amounts are raw wei strings.

---

### `GET /pad/tokens/:address/traders`

All wallet addresses that have traded a token.

```json
{
  "traders": ["0x...", "0x...", "0x..."]
}
```

Note: This reflects addresses that have traded, not current ERC-20 holders.

---

### `GET /pad/stats`

Aggregate launchpad statistics.

| Param | Type | Description |
|-------|------|-------------|
| `chain` | `mainnet` \| `testnet` | Filter by network |

```json
{
  "totalTokens": 42,
  "activeTokens": 35,
  "graduatedTokens": 7,
  "totalVolume24h": "1234.56",
  "totalTrades24h": 89
}
```

---

### `GET /pad/leaderboard`

Top traders by realized + unrealized PnL.

| Param | Type | Description |
|-------|------|-------------|
| `chain` | `mainnet` \| `testnet` | Filter by network |
| `limit` | number | Default: 10 |

---

### `GET /pad/traders/:address/positions`

Per-token positions for a wallet, including cost basis and PnL.

| Param | Type | Description |
|-------|------|-------------|
| `chain` | `mainnet` \| `testnet` | Filter by network |

---

## BlumeSwap (DEX)

### `GET /dex/pools`

All liquidity pools with current reserves.

| Param | Type | Description |
|-------|------|-------------|
| `chain` | `mainnet` \| `testnet` | Filter by network |

**Response:**

```json
{
  "pools": [
    {
      "address": "0x...",
      "token0": { "address": "0x...", "symbol": "TKN", "name": "Token", "decimals": 18 },
      "token1": { "address": "0x...", "symbol": "WXRP", "name": "Wrapped XRP", "decimals": 18 },
      "reserve0": "50000000000000000000000",
      "reserve1": "1000000000000000000000",
      "totalSupply": "7071067811865475244",
      "spotPrice": "0.02",
      "chainId": 1440000,
      "updatedAt": "2026-03-01T12:00:00.000Z"
    }
  ]
}
```

Reserves are raw wei strings. `spotPrice` is `reserve1 / reserve0`.

---

### `GET /dex/pools/:address`

Single pool detail with recent events.

---

### `GET /dex/pools/:address/events`

Paginated swap, mint, and burn events for a pool.

| Param | Type | Description |
|-------|------|-------------|
| `limit` | number | Max 100 |
| `cursor` | string | Pagination cursor |
| `type` | `swap` \| `mint` \| `burn` | Filter by event type |

---

### `GET /dex/tokens`

All ERC-20 tokens the DEX indexer has seen.

| Param | Type | Description |
|-------|------|-------------|
| `chain` | `mainnet` \| `testnet` | Filter by network |

---

## WebSocket

**URL:** `wss://api.blumefi.com/ws`

Subscribe to real-time events by sending JSON messages.

### Subscribe

```json
{ "type": "subscribe", "channel": "pad" }
```

### Channels

| Channel | Required params | Description |
|---------|----------------|-------------|
| `pad` | — | All Blumepad events (trades, launches, graduations) |
| `pad_token` | `address` | Events for a specific token |
| `bridge` | — | All bridge relay events |
| `bridge_user` | `address` | Bridge events for a specific wallet |

### Event types

**`pad_trade`** — Emitted on every token buy/sell.

```json
{
  "type": "pad_trade",
  "channel": "pad",
  "data": {
    "tokenAddress": "0x...",
    "tradeType": "buy",
    "trader": "0x...",
    "xrpAmount": "10000000000000000000",
    "tokenAmount": "8130000000000000000000",
    "progressPct": 67.5,
    "marketCapXrp": "1234.56",
    "priceXrp": "0.00123",
    "chainId": 1440000
  }
}
```

**`pad_graduated`** — Emitted when a token graduates to the DEX.

```json
{
  "type": "pad_graduated",
  "channel": "pad",
  "data": {
    "address": "0x...",
    "pairAddress": "0x...",
    "chainId": 1440000
  }
}
```

### Unsubscribe

```json
{ "type": "unsubscribe", "channel": "pad" }
```
