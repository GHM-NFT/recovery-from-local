Smart contract & chain choices (plain-English + what to add)
ERC-721 vs ERC-1155
	•	What:
	◦	ERC-721 = unique tokens (1/1s).
	◦	ERC-1155 = editions (many copies share one metadata/ID).
	•	When to use:
	◦	Mythic Icon 1/1 → ERC-721
	◦	Signature Editions of 8 / 10K video variant → ERC-1155
	•	Add to Control sheet: standard (721/1155), edition_size, token_range
Royalties (EIP-2981)
	•	What: A standard so marketplaces know your royalty rate (e.g., 7.5%).
	•	Caveat: Enforcement varies by marketplace; still set it for clear intent.
	•	Add: royalty_bps (e.g., 750 = 7.5%), royalty_recipient (address)
Chain / L2 selection
	•	Choices: Ethereum (prestige, higher gas), Base or Polygon (lower fees).
	•	Approach: You can mix per collection if needed.
	•	Add: chain (ETH/Base/Polygon), est_gas_mint, est_gas_batch
Operator filter (listings compatibility)
	•	What: Some contracts restrict certain marketplaces; decide your policy.
	•	Recommendation: Use broad compatibility unless you have a reason to restrict.
	•	Add: operator_filter (on/off) + a short policy note.
Freeze metadata (immutability)
	•	What: “Locking” token metadata/CIDs after release.
	•	Why: Collector trust; but you lose flexibility for fixes.
	•	Plan: Freeze after your QA window.
	•	Add: freeze_policy (when), freeze_date (planned)
Mint platform / contract provenance
	•	What: Will you deploy via Manifold, Zora, OpenSea, or a custom contract?
	•	Tip: Use a single canonical collection address per series for brand coherence.
	•	Add: contract_factory (Manifold/Zora/Custom), collection_address, collection_slug
Allowlist & claims (if used)
	•	What: Pre-approved wallets, per-wallet caps, claim windows.
	•	Add: allowlist_source (CSV/Merkle), claim_start, claim_end, per_wallet_cap, reserve_supply

Quick column insert suggestions
In Control sheet:
	•	standard | chain | royalty_bps | royalty_recipient | operator_filter | freeze_policy | freeze_date | contract_factory | collection_address | collection_slug | allowlist_source | claim_start | claim_end | per_wallet_cap | reserve_supply
In Manifest (editions):
	•	edition_size | token_range
