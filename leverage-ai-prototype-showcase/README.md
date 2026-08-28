# Leverage AI: Missed-Call Recovery Prototype

**[Full technical case study →](https://claude.ai/code/artifact/4673b74b-e61d-4914-8d1d-b514a10c6091)**

**What it is:** a backend automation service for home-service businesses (HVAC, plumbing, electrical) that recovers revenue lost to missed phone calls. When a call goes unanswered, the system texts the caller back within seconds, has an AI agent gather the details a human dispatcher would ask for, and places a placeholder booking on the business's calendar for a human to confirm.

This repository documents the architecture and design decisions. The source is closed, since this is active infrastructure behind a real, ongoing business (Leverage AI / MLEbotics), not a static demo, and there's no public app or endpoint to link here since it's a backend service driven entirely by phone-carrier webhooks rather than a UI.

## The problem

Home-service businesses lose a meaningful share of leads to missed calls: a technician mid-job can't answer the phone, and most callers who reach voicemail simply call the next number on the list instead of leaving a message. Leverage AI turns a missed call into an SMS conversation, immediately, so the lead isn't handed to a competitor by default.

## How it works

1. **Missed call detected.** Twilio's Voice status callback fires only on `no-answer` / `busy` / `failed`, not on every answered call, and hits a webhook that immediately texts the caller.
2. **AI-driven qualification over SMS.** Every reply from the customer is routed through Claude, using a *forced tool call* rather than free-form text generation: the model must return a structured decision (conversation status, extracted service type / urgency / location / requested time) on every turn, not prose it might phrase inconsistently. This makes the conversation state machine reliable to drive downstream logic against.
3. **Deliberately narrow scope.** The agent does exactly three things: acknowledge, qualify, decide the next step. It's instructed to never quote a price, never promise a specific arrival window, and to escalate immediately (alerting the business owner directly by SMS) on anything ambiguous or an actual emergency (gas smell, flooding, no heat in freezing weather, etc.) rather than attempt to handle it. Keeping the model's authority narrow is what keeps a fully-automated intake trustworthy enough to run unattended.
4. **Placeholder booking, not a real one.** Once qualified, a calendar event is created via the Google Calendar API, clearly marked "NEEDS CONFIRMATION," rather than the AI silently locking in a real appointment slot it has no way to verify availability for. A human reviews and confirms the actual time.
5. **Owner stays in the loop.** Emergencies, out-of-scope questions, and successful bookings all trigger an SMS alert directly to the business owner, so nothing requiring judgment gets stuck waiting on the AI.

## Architecture

| Layer | Tech |
|---|---|
| Webhook server | Node.js + Express |
| Call/SMS channel | Twilio (Voice status callbacks + Messaging webhooks) |
| Lead qualification | Anthropic Claude, via a forced tool-use call (structured output, not free text) |
| Calendar integration | Google Calendar API (OAuth 2.0, refresh-token flow) |
| Conversation state | In-memory per-conversation store, keyed by phone number |

## Design decisions worth calling out

- **Tool-use over prompted JSON.** Forcing the model to call a single defined tool (`respond_to_lead`) rather than asking it to "return JSON" in a system prompt eliminates an entire class of parsing failures and keeps the extracted fields (status, service type, urgency, location, requested time) type-safe by construction.
- **The AI is a filter, not a decision-maker, on anything consequential.** Pricing, exact scheduling, and complaints are explicitly out of its authority: they escalate to a human. This is a deliberate reliability/trust tradeoff: a narrower agent that always hands off the hard cases beats a broader one that occasionally guesses wrong on something that matters.
- **Never invent availability.** The calendar integration books a clearly-labeled placeholder rather than treating the AI's read of "tomorrow afternoon works" as a locked slot. The business owner is always the one who confirms real availability.

## Status

Active prototype, in use for real customer outreach as of August 2026. Not open-sourced, since it's live business infrastructure rather than a portfolio artifact. This write-up exists so the design and engineering can be evaluated without exposing what's still an active sales pipeline.
