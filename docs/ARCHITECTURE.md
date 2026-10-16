# Free Exchange Architecture

## Layers
- Web UI: quote, order creation, QR payment, status, history.
- API: quote/order endpoints and order state transitions.
- Database: orders, payments, confirmations, payouts, assets/networks and idempotency records.
- Chain watchers: network-specific payment detection and confirmation tracking.
- Quote engine: market price, fee, slippage/rate-lock rules and expiry.
- Settlement engine: payout intent, broadcast, confirmation and retry/reconciliation.
- Admin: operational order review, liquidity and exception handling.

## Important boundary
The UI and API must not infer user identity from a connected wallet. An order is the primary business identity. Destination address is order data, not an authenticated account.

## Network abstraction
Asset and network are separate fields. The same asset symbol may exist on multiple networks and must never be treated as interchangeable.
