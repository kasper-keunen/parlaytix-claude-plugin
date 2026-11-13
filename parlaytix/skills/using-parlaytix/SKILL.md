---
name: using-parlaytix
description: Use when someone wants ParlayTix fan tickets (find, buy, sell, request or gift), a Scout watch, prediction-market odds or research, their saved positions or plans, or a Group Wager with friends.
---

# Using ParlayTix

ParlayTix is the prediction market for tickets. A fan ticket becomes a real ticket when its condition hits, such as the fan's team reaching the final. The `parlaytix` tools read and prepare; the fan confirms anything that moves money or ownership on ParlayTix, signed into the same account.

## Every job

- **Handoff.** A tool that prepares a purchase, listing, request, cancellation, gift or paid watch step returns a link (`confirmUrl`, `paymentUrl`, `roomUrl` or a `handoff`). Give the fan that exact link and say what they will confirm there. The call itself moves no money and changes no ownership.
- **Receipt.** Say paid, owned, listed, filled, refunded or delivered only from the matching read's receipt or final state. Prepared, pending, submitted and confirming are in progress; report them as in progress.
- **Exact identity.** Pass IDs, listing versions, outcome IDs and cursors whole, exactly as returned. After a timeout or unclear response, retry with the same `idempotencyKey` or `intentId` and the same terms.
- **Coverage.** Carry partial-coverage, stale-price and warning fields into the answer. A provider error or partial page is unknown, separate from a completed empty search.
- **Permissions.** Each job has its own consent, and one never implies another. When a tool refuses for a missing permission, call `get_connection_permissions` and point the fan to [AI agents](https://parlaytix.xyz/account?tab=agents) to allow it.
- **Words.** Say fan ticket, listing price and watch. Keep claim, collateral and other internal field names out of the answer.

## Jobs

**Buy a fan ticket.** `search_ticket_listings` (or `search_ticket_events` to find the event) → `get_ticket_asset` for the condition, listing price and price freshness → once the fan picks, `create_ticket_checkout` with that listing's current ID and version → give them `confirmUrl` → `get_ticket_checkout` until its receipt is completed. Before they confirm, state the deal: the payment is upfront and at risk if the condition misses; a hit gets the real ticket, claimed from the card and delivered by email. On `LISTING_CHANGED`, re-read the card and get a fresh yes on the new terms.

**Sell a card.** `list_my_ticket_assets` → `prepare_ticket_listing` → `confirmUrl` → `get_ticket_asset` shows the live listing once the fan confirms.

**Request a ticket.** `search_ticket_events` for the target → `search_request_questions` for the exact condition and `outcomeId` → `get_request_board` or `get_request_demand` for context → `prepare_ticket_request` → handoff → `list_my_ticket_requests` for deposit, fill and refund state.

**Gift a card.** Only when the fan asks to give or accept a specific card. The gift link and code are credentials: show them to the sender alone, in this conversation.

**Scout watch.** A watch looks for tickets to one event inside the fan's limits. Restate the terms (event, quantity, cap, deadline, condition, approval mode) and get a yes before `create_scout_intent`; `activate_scout_intent` starts it. `get_scout_intent` shows saved status, decisions and any pending approval, answered with `respond_to_scout_intent_approval`. These reads show saved state, not whether the watch is checking right now, and a saved decision is not ticket delivery.

**Prediction markets.** `search_prediction_markets` → `get_prediction_market_odds` or `research_prediction_market` for one exact market. Report odds with their source time and say when current data is unavailable. Describe the evidence and leave the decision to the fan; these tools place no trades. Saved positions and plans are stored records, not live valuations.

**Group Wager.** An honor bet between friends: stakes are symbolic and ParlayTix holds no money. `create_group_wager_draft` and `update_group_wager_draft` shape the terms; agreeing, inviting, voting and the outcome all happen in the room at `roomUrl`.

Tools not named here, such as packs and splits, carry their rules in their own descriptions. Pack authoring is for company operators.
