# Free Exchange Product Specification

## Core flow
1. Select source asset/network and destination asset/network.
2. Enter source amount.
3. Enter destination wallet address.
4. Select Fixed Rate or Floating Rate.
5. Create an order and display payment QR.
6. Detect the blockchain payment.
7. Wait for required confirmations.
8. Calculate final output amount.
9. Send destination asset directly to the supplied destination address.
10. Persist payment and payout transaction hashes and final status.

## Fixed Rate
- Quote is locked for 10 minutes.
- The user must pay the exact order amount within the validity window.
- Late, underpaid, overpaid, wrong-network and wrong-asset payments must enter exception handling rather than silently settling.
- Initial product fee target: 1% plus network fee.

## Floating Rate
- Quote shown before payment is indicative.
- Final rate is determined after the payment reaches the required confirmation threshold.
- Initial product fee target: 0.5% plus network fee.

## Order state machine
CREATED -> WAITING_PAYMENT -> PAYMENT_DETECTED -> CONFIRMING -> CONFIRMED -> PROCESSING -> COMPLETED

Exception states: EXPIRED, UNDERPAID, OVERPAID, WRONG_ASSET, WRONG_NETWORK, REFUND_REQUIRED, REFUNDED, FAILED.

## Reliability requirements
- Idempotent payment detection.
- One on-chain payment can settle at most one order.
- Payout intent must be persisted before/around payout execution so RPC timeouts cannot cause duplicate payouts.
- Payment, confirmation and payout processing must be retryable.
- Never treat an RPC timeout as proof that a payout failed.
- Every order must retain payment TX and payout TX when available.

## Privacy / wallet model
The application does not connect to the user's wallet. There is no WalletConnect, injected-wallet authorization, or user asset balance. The user supplies only the destination address needed for the payout.
