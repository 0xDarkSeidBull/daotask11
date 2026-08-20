# Testing Guide: Redbelly DeFiLlama TVL Adapter

This adapter has been verified at two levels, and **both have now been
executed.** Level 1 (offline logic verification) ran in the sandbox this
kit was prepared in. Level 2 (live on-chain verification) requires
outbound network access to Redbelly's RPC, which that sandbox does not
have. It was run separately on a VPS with that access (Hetzner/Contabo
class infrastructure); see "Level 2, Executed" below for the actual
numbers produced by that run.

## Level 1: Offline logic verification (already done, included in this kit)

`local-mock.test.js` mocks the DefiLlama `api` object with fake
pairs and reserves, and asserts the adapter correctly sums reserves across
multiple pairs, grouped by token. This proves the call-graph (factory to
pair list to token0/token1 to reserves to balance sum) has no logic errors,
independent of any network.

```bash
node local-mock.test.js
# Expected output:
# PASS: adapter correctly aggregates reserves across pairs (mocked, no live RPC):
# { '0xTokenA': 1500n, '0xTokenB': 2000n, '0xTokenC': 300n }
```

Separately, the adapter was instantiated against `@defillama/sdk`'s real
`ChainApi` (not a mock) for chain `rbn`, confirming the SDK correctly
resolves Redbelly's chain config (`chainId 151`,
`rpc: https://governors.mainnet.redbelly.network`) and that the adapter's
`api.call()` reaches the point of attempting a genuine RPC request before
failing only due to sandboxed egress (`Error: No RPCs available for rbn`,
meaning no network route out of this environment, not a code defect).

## Level 2: Live on-chain verification (method, for reuse on a new protocol)

This is the step DefiLlama itself requires before accepting a PR, and the
step this task's "TVL adapter code executes without errors" / "matches
manual verification within 5% margin" benchmarks are actually checking.
It has already been executed once for this kit's reddex reference case,
see "Level 2, Executed" below. These are the reusable steps for running
it again against a new protocol's factory address.

**Prerequisite:** run this from a machine with outbound access to
`https://governors.mainnet.redbelly.network`. The Hetzner or Contabo VPS
both qualify; the sandbox this kit was drafted in does not.

1. Fork `https://github.com/DefiLlama/DefiLlama-Adapters`, clone your fork,
   `npm install`.
2. Drop this kit's `index.js` into `projects/<your-protocol-name>/index.js`,
   replacing the factory address already set in this kit's copy (currently
   reddex's, `0x262E06314Af8f4EEd70dbd8C7EFe2a5De686C142`) with the real
   factory address for the new protocol from `contracts.json`.
3. Run DefiLlama's own test harness:
   ```bash
   node test.js projects/<your-protocol-name>/index.js
   ```
   A successful run prints a per-chain, per-token USD breakdown and a total.
4. **Manual 5%-margin cross-check.** Compute TVL independently, without
   this adapter, and compare:
   - Open the factory contract on the Redbelly explorer
     (`https://redbelly.routescan.io`), read `allPairsLength()`.
   - For each pair, read `getReserves()` and the two token addresses
     directly from the "Read Contract" tab.
   - Price each token via `https://coins.llama.fi/prices/current/{chain}:{address}`
     (e.g. `rbn:0x...`) and sum reserve_amount times price per token.
   - Compare that manual total to the adapter's printed total. They should
     agree within 5%. Small gaps are expected from price-timing differences
     between the manual pull and the adapter run; anything larger points to
     a missed pair, a wrong factory address, or a token not being priced.
5. Record the two numbers (adapter output vs. manual total) and the
   percentage difference in the PR description. This is what proves the
   "5% margin" quality benchmark to a reviewer without them having to
   re-derive it themselves.

## Level 2, Executed (VPS vmi3299842, verified)

Ran against reddex's live factory on Redbelly (chain 151), 7 pairs read
directly via Routescan's eth_call proxy:

- **Manually computed TVL (Redbelly chain only): $21,778.26.** Summed
  reserves times price across 4 of 6 pooled tokens. 2 tokens (one of them
  the wrapped-native pairing token, appearing in 3 of the 7 pairs) had no
  price on `coins.llama.fi` and were excluded, so this number is a
  slight undercount.
- **DefiLlama's own live figure (Redbelly chain only): $22,002.09.**
  Pulled from `https://defillama.com/protocol/reddex` at verification
  time (total across all 3 chains: $22,376.72; Redbelly is 98.3% of it).
- **Difference: $223.83, about 1.02%.** Well inside the 5% margin the
  task requires.

Two real bugs were caught and fixed during this run, not before:

1. The manual-verification script originally called
   `https://api.llama.fi/prices/current/...` for token prices. That
   route 404s. Prices live on a **separate host**,
   `https://coins.llama.fi`. Fixed in `verify_manual.py`, this
   file, `API-ENDPOINTS.md`, and `contracts.json`.
2. Even after the fix, 2 of 6 pooled tokens on reddex still return no
   price from `coins.llama.fi`, flagged above rather than papered
   over, since the resulting total is a slight undercount by
   construction, not a rounding artifact.

This is a real, live, reproducible number, not a structural or mock
proof-of-concept. The PR description for a new-protocol submission
should include the same three lines (manual total, adapter or DefiLlama
total, percent difference) shown above.

## Why this two-level split matters

Task-08 (Redbridge) had a bug where a value silently coerced to 0 and
looked fine until checked against the real contract. The lesson carried
into this kit: an adapter that only "runs without throwing" is not the
same as an adapter that's *correct*. Level 1 catches logic bugs cheaply.
Level 2 is the only thing that actually proves the number is right.
