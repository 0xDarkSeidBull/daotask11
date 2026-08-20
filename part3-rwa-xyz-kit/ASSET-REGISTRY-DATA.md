# Asset-Registry Data: RWA.xyz Format

RWA.xyz's own Asset data schema (`docs.rwa.xyz/schemas/assets.md`) is
what their Partners App ultimately stores an asset as, and is the
canonical field list referenced by their "Asset Reference Data" step
(Step 3 of `ONBOARDING-CHECKLIST.md`). This file pre-fills that schema's
core fields for each Redbelly-ecosystem asset/platform this kit could
independently confirm (see `PLATFORMS-TO-INTRODUCE.md` for the sourcing
on each). Fields the schema defines but that need an internal decision
or platform-side confirmation are marked **[CONFIRM WITH TEAM]**, the
same convention `NETWORK-PROFILE-DRAFT.md` uses; fields are not left
blank or guessed.

Per RWA.xyz's own onboarding docs, Asset Reference Data is submitted by
the *platform or issuer*, not by the network. This data is prepared so
each introduction in Step 3 hands the platform a filled template instead
of a blank form.

---

## 1. wisr Australia: Freedom Trust

| RWA.xyz field | Value |
|---|---|
| `name` | Freedom Trust |
| `description` | RBA Smart ABS Pilot asset-backed securities trust, structured credit issued by wisr Australia, settled on Redbelly Network. |
| `asset_class_name` | Private Credit (confirm exact taxonomy bucket with wisr, see note in `PLATFORMS-TO-INTRODUCE.md`; may map to "Institutional Alternative Funds" instead) |
| `issuer_name` | wisr Australia |
| `network_names` | Redbelly Network |
| `jurisdiction_country_name` | Australia |
| `regulatory_framework` | Reserve Bank of Australia (RBA) Smart ABS Pilot |
| `total_asset_value_dollar` | $250,000,000 (per Redbelly's own X/Twitter announcement; **[CONFIRM WITH TEAM]** against wisr's own disclosure before submitting, since this is a regulator-pilot figure) |
| `has_native_tokens` | **[CONFIRM WITH TEAM]**, not independently verifiable without wisr's cooperation |
| `is_investable` | **[CONFIRM WITH TEAM]** |
| `traditional_custodian` / `crypto_custodian` | **[CONFIRM WITH TEAM]** |
| `website` | **[CONFIRM WITH TEAM]** |
| Source | Redbelly Network's own X/Twitter account (see `PLATFORMS-TO-INTRODUCE.md` #2 for full sourcing and caveats) |

## 2. Hutly

| RWA.xyz field | Value |
|---|---|
| `name` | Hutly |
| `description` | Property management platform issuing/managing tokenized real-estate-adjacent assets on Redbelly Network. |
| `asset_class_name` | Real Estate |
| `issuer_name` | Hutly |
| `network_names` | Redbelly Network |
| `jurisdiction_country_name` | **[CONFIRM WITH TEAM]** (Hutly is referenced as an Australian property-management platform in Redbelly's blog, but this report did not independently verify incorporation jurisdiction) |
| `is_investable` | **[CONFIRM WITH TEAM]**. Blog reference is a passing example, not a dedicated listing announcement (see `PLATFORMS-TO-INTRODUCE.md` #3); confirm Hutly is issuing live, not piloting, before marking investable |
| `total_asset_value_dollar` | **[CONFIRM WITH TEAM]**, no figure published |
| `website` | **[CONFIRM WITH TEAM]** |
| Source | Redbelly Network Blog, "Not Another RWA 101," 20 March 2025 |

## 3. Raze Finance

Raze is tokenization *infrastructure*, not a single asset. Per RWA.xyz's
schema this is a **Platform** record, not an Asset record (Platform
Reference Data, not Asset Reference Data). Included here because it's
the entry point for whatever individual assets get issued through it.

| RWA.xyz field (Platform record) | Value |
|---|---|
| `name` | Raze Finance |
| `description` | No-code RWA tokenization engine; handles token creation, issuance, and investor management, with Redbelly providing settlement/compliance. |
| `network_names` | Redbelly Network |
| Listing Type | Protocol (per `docs.rwa.xyz/onboarding/platforms`, platforms register as "Protocol"; issuers register as "Issuer") |
| Individual asset records | Not yet known, depends on what Raze has issued through the partnership; to be collected once Raze is introduced (Step 3) |
| Source | EIN Presswire / WJBF, 11 April 2025 partnership announcement. **[CONFIRM WITH TEAM]** the partnership is still active before introducing, per the caveat already in `PLATFORMS-TO-INTRODUCE.md` #1 |

---

## What's deliberately not filled in

Fields present in RWA.xyz's Asset schema that depend on data only the
platform/issuer holds directly: `isin`, `cusip`, fee structure
(`total_management_fee_bps`, etc.), custody/service-provider IDs
(`traditional_custodian_id`, `auditor_id`, `transfer_agent_id`, etc.),
and all live on-chain metrics (`total_supply_token`,
`holding_addresses_count`, transfer/bridge volumes) are intentionally
left as **[CONFIRM WITH TEAM]** or omitted rather than estimated. RWA.xyz
computes the on-chain metrics themselves once a network is integrated
(Step 4); hand-submitting guessed numbers here would create data that
has to be corrected later rather than saving anyone time now.

*Source: `docs.rwa.xyz/schemas/assets.md` (RWA.xyz's own published Asset
data field reference, fetched and field-matched against this task's
independently-confirmed platform data, see `PLATFORMS-TO-INTRODUCE.md`
for each source).*
