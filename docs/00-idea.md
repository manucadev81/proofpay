# ProofPay: an AI agent that pays APIs per request and only keeps paying providers that deliver

Status: awaiting approval (2026-11-23). Working name: ProofPay. Owner account to be confirmed before the repository is created.

## Problem

Developers building AI agents need them to pay for APIs (data, search, inference) per request, without giving the agent a card or a prepaid account per provider. Pay-per-request standards such as x402 and agent wallets with spending caps (for example Circle Agent Wallet policies, smart-account session keys) already cap how much an agent can spend. None of them check whether what the agent paid for was actually delivered: an agent keeps paying a provider that returns errors, slow answers or responses that do not match what was promised, until a person notices.

## Audience

Developers who run AI agents that call paid APIs. They configure the agent's budget and policy and need an auditable record of what was paid, to whom and why.

## Why blockchain

- The agent pays providers it has no account with, per request, in a stablecoin; settlement onchain removes the card, the signup and the prepaid balance per provider.
- Spending caps, the provider allowlist and blocklist and the quality record live in a contract, so they hold even if the agent's off-chain process is compromised, and anyone can audit them on a block explorer.
- What breaks without it: the same flow is possible with API keys and prepaid credits, but each provider needs an account, caps depend on each provider's dashboard, and there is no shared, verifiable record of spend and quality.

## Why this chain

Chosen by the target events, not by preference:

- Aevum testnet (QDay, EVM-compatible, post-quantum) with the tUSD8 test stablecoin, required by the Aevum Builder Challenge #1, which explicitly scores spend caps, session keys, smart accounts, agentic payment primitives and onchain audit trails.
- EVM is kept for the second event (ETHGlobal Agentic Markets), where the leading sponsor (Hedera) is EVM-compatible; the sponsor's requirements were not published as of 2026-11-23.

## Target program and competitors

- Aevum Builder Challenge #1 (DoraHacks): build period 2026-11-16 to 2026-12-11 08:00 UTC; all code written during the build window; contracts on Aevum testnet with test tokens only; public repository, live deployment, documentation; top 5 present on 2026-12-15; US$ 1,500 in USDT.
- ETHGlobal Agentic Markets: async, 2026-12-03 to 2026-12-10; application required ("Apply to attend"); existing code allowed ("Extend Open Source"); sponsor prizes Hedera US$ 15,000, Uniswap Foundation US$ 5,000, ENS US$ 5,000, details pending; top 10 finalists receive 1,000 USDC per member.
- Neighbors: x402 (pay per request over HTTP), Circle Agent Wallet spending policies, smart-account session keys (ERC-4337 tooling). They cap spend; ProofPay adds pay-for-value: spend follows delivered quality.

## MVP scope

1. Agent wallet as a smart account with a session key and onchain policy: per-request cap, daily cap, per-provider cap, provider allowlist and blocklist, instant revocation.
2. x402-style flow: a paid API answers 402 with a price; the agent pays in tUSD8 and retries with proof of payment; a demo provider server is included.
3. Value check after each paid response: status, latency and response schema against what the provider declared. Each result is recorded onchain as a quality attestation for that provider.
4. Pay-for-value rule: after a configurable number of failed checks, the contract blocks the provider automatically; budgets shift to providers that deliver.
5. Developer view: live spend, caps, provider scores, blocked providers and explorer links for every payment and attestation.
6. Agent identity registered with ERC-8004, which the Aevum challenge lists as an integration signal.
7. Demo with three demo providers (good, slow, broken) so the blocking is visible in under five minutes.

## Out of scope

- Mainnet and real funds.
- Refunds or escrow of payments already made (a failed request is paid; the rule changes future payments).
- Multiple agents sharing one budget.
- Algorand and non-EVM chains.
- Provider-side marketplace or listing.
- Any reuse of code written before 2026-11-23 in the Aevum submission.

## Risks

- Timing: 18 days to the Aevum deadline, with ETHGlobal (2026-12-03 to 12-10) overlapping the last week.
- Rule risk: Aevum requires all code written in the build window; ETHGlobal reuse must come after, as an extension. Aevum's originality criterion may weigh against submitting the same project to two events.
- Trust: the value check runs in the agent's off-chain process; the contract trusts the attestations it signs. Mitigation: caps and revocation stay onchain regardless of attestations.
- Dependency: Aevum testnet stability and tooling (starter kit on Discord); ETHGlobal sponsor requirements unknown and may not fit.
- Adoption: x402 and agent wallets evolve fast; a provider may add quality guarantees natively.

## Success metric

- By 2026-12-11: Aevum submission complete, with live testnet deployment, repository, documentation and a demo showing the broken provider blocked automatically, with explorer links.
- Selected in the Aevum top 5 (Demo Day 2026-12-15).
- By 2026-12-10: ETHGlobal submission as an extension, fitted to at least one sponsor prize once requirements are published.

## Open questions

- Owner account (personal or Nearx-Labs) and whether the repository is public from the start (the Aevum challenge requires a public repository).
- ETHGlobal sponsor requirements, especially Hedera's; adapt or skip the sponsor prize when published.
- Which Aevum native tooling exists for smart accounts and session keys (starter kit), or whether a standard ERC-4337 setup is used.
- Final project name.
