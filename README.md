# TASK-11: RBNT Token Utility and Ecosystem Visibility Report

<img width="1434" height="1024" alt="dao-logo-on-dark" src="https://github.com/user-attachments/assets/a1bf671a-53fa-42ca-bb86-1ffc26176e07" />

Submitted deliverable for the Redbelly DAO Task Board. Four parts, every figure traced to a primary source: official whitepaper, live on-chain data, or the platform's own docs.

**Status:** revision requested on the first review. All six requested fixes are addressed below and verified against this repo's actual current files. Ready for resubmission.

## Quick links

| | |
|---|---|
| Live showcase site | https://daotask11.test-hub.xyz/ |
| Tokenomics Report, read in browser | [PDF](https://cdn.jsdelivr.net/gh/0xDarkSeidBull/daotask11@main/RBNT_Token_Utility_Report_1.pdf) / [Docs](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/0xDarkSeidBull/daotask11/main/RBNT_Token_Utility_Report_1.docx&embedded=true) |
| Explainer article | https://dev.to/0xdarkseidbull/verifying-before-shipping-a-rbnt-token-case-study-3bc1 |

## The four deliverables

1. [Tokenomics Report (PDF/DOCX)](./part1-tokenomics-report/): whitepaper-cited, 12 pages. Covers all 5 real token uses, full allocation table, vesting timeline, and a 4-way competitor comparison.
2. [DeFiLlama Submission Kit](./part2-defillama-kit/): TVL adapter for reddex, live-verified against DeFiLlama's own numbers (about 1% margin, well inside the 5% accuracy requirement).
3. [RWA.xyz Submission Kit](./part3-rwa-xyz-kit/): onboarding checklist, pre-filled network profile, asset-registry data in RWA.xyz's own schema format, and a platform-introduction list for RWA.xyz's manual partnership process. Overall application timeline (4 to 8 weeks, per RWA.xyz's own FAQ) is documented in `ONBOARDING-CHECKLIST.md`.
4. [Explainer Article](./part4-explainer/): "How Network Adoption Drives RBNT Value," under 500 words, written for Discord and social, also published on Dev.to (link above).

Reviewer note: each part's own file explains its verification method and any caveats found along the way, for example the domain bug fixed in Part 2, and the DeFiLlama-vs-RWA.xyz absence distinction in Part 3.

## What changed since the last review

The first review requested six fixes. All six are done:

1. **Explainer title and length.** Retitled to the exact required "How Network Adoption Drives RBNT Value," rewritten under 500 words arguing that adoption drives RBNT demand, and republished on Dev.to at the link above.
2. **DeFiLlama adapter placeholder.** `part2-defillama-kit/index.js` now has reddex's real factory address wired in (sourced from `contracts.json`), not the `FACTORY_ADDRESS_HERE` placeholder. The adapter executes.
3. **XDC supply figure.** Table 7.1 in the Tokenomics Report now states XDC has no maximum supply and that the approximately 37.5B pre-mine is a genesis balance, not a cap, sourced from `docs.xdc.network`. Section 8.3 no longer claims no official XDC tokenomics page exists.
4. **RWA.xyz asset-registry data and timeline.** `part3-rwa-xyz-kit/ASSET-REGISTRY-DATA.md` adds pre-filled Asset Reference Data in RWA.xyz's own schema format, and `ONBOARDING-CHECKLIST.md` states the 4 to 8 week end-to-end application timeline from RWA.xyz's own FAQ.
5. **Internal path references.** Every reference in `TESTING.md`, `API-ENDPOINTS.md`, `SUBMISSION-GUIDE.md`, and the `index.js` header comment now points at a file that actually exists in this flat kit, or is clearly labeled as describing DefiLlama's own external repo structure rather than this kit's own.
6. **TESTING.md contradiction.** The document now states plainly that Level 2 (live on-chain verification) could not run inside the drafting sandbox, was run separately on infrastructure with RPC access, and presents that run's real results under "Level 2, Executed," rather than leaving it ambiguous whether live verification happened.

## Sources and verification

Every figure in every part traces back to a primary source, cited inline in that part's own files: Redbelly's official whitepaper (August 2025), live DeFiLlama API responses, RWA.xyz's own published docs and schema, and XDC Network's own documentation. Where this task's research corrected something in the original brief (the RWA.xyz-vs-DeFiLlama absence claim, the DeFiLlama price-endpoint domain, the XDC supply figure), that correction is explained in place rather than applied silently.
