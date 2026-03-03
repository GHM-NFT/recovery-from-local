Final 5-Tier Structure (per deity) — White-paper spec (clean + implementable)
Tier 1 — Mythic Icons
	•	Purpose: The single, highest-status “flagship” work for that deity (museum-grade, canonical).
	•	Typical token standard: ERC-721 (1/1)
	•	Media: High-resolution image (optionally paired with a hero video, but usually image-led).
	•	Collector promise: Maximum scarcity + maximum narrative weight (the “cover” piece).
	•	Metadata cues: Tier = Mythic Icon; edition_size = 1; strongest/most complete trait set; premium frame.
Tier 2 — Signature Editions
	•	Purpose: The “core collectible set” for the deity—still premium, but accessible vs the 1/1.
	•	Typical token standard: ERC-1155 (small editions) (or ERC-721 if you want each variant as a unique 1/1)
	•	Media: Image-led; can include select motion variants.
	•	Collector promise: The best balance of scarcity, price-access, and completionism.
	•	Metadata cues: Tier = Signature Edition; edition_size = N; consistent naming convention across variants (Poseidon / Poseidon Variant / Frame).
Tier 3 — Companion Pieces
	•	Purpose: Supporting artworks that expand the deity’s world: symbols, scenes, artifacts, secondary compositions.
	•	Typical token standard: ERC-1155 (mid editions) or ERC-721 if each companion is unique.
	•	Media: Mostly images; occasional motion.
	•	Collector promise: Completion + lore-building; designed to “stack” with the Signature narrative.
	•	Metadata cues: Tier = Companion Piece; traits emphasize symbols, story fragments, locations, or myth_scene.
Tier 4 — Limited Edition Prints
	•	Purpose: Bridge between digital collecting and physical fine-art ownership.
	•	Typical token standard: Usually ERC-1155 (because it maps cleanly to print runs).
	•	Media: Image NFT + print entitlement (redeemable or non-redeemable, depending on your ops).
	•	Collector promise: Clear edition cap tied to physical production.
	•	Metadata cues: Tier = Limited Edition Print; include print specs in unlockables/SoT: paper, size, run size, COA handling.
Tier 5 — Relics (all videos)
	•	Purpose: Motion-only collectible layer: cinematic loops, myth fragments, “relic recordings,” trailer-style pieces.
	•	Typical token standard: ERC-1155 (unless each relic is truly unique, then ERC-721).
	•	Media: Video only → use animation_url / animation_cid as the primary asset; optional poster image.
	•	Collector promise: A distinct medium category (video) that complements the static tiers and can power quests/journeys.
	•	Metadata cues: Tier = Relic; animation_url present on all; durations/format consistent; optional “Relic Type” trait (Loop / Fragment / Chronicle / Beacon).

Practical implementation mapping (so Codex/dev can’t misread it)
	•	ERC-721: Mythic Icons (1/1), optionally “hero” 1/1 variants elsewhere.
	•	ERC-1155: Signature Editions, Companion Pieces, Limited Edition Prints, Relics (videos).
	•	Asset fields:
	◦	Images → image (from cid_Poster_Image / image_cid)
	◦	Videos → animation_url (from animation_cid) + optional poster in image
	•	One consistent trait: Tier with exactly these 5 values (spelling frozen).
