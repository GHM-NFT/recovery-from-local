Google Doc To Local Folder Pipeline + Master manifest is the source of truth - Codex handoff document (spec + artifacts)

A. Objectives
Create a reliable, repeatable pipeline where:
	•	Master Manifest is the sole authoritative data source.
	•	All generated outputs in GHM_NFT_PROJECT can be regenerated deterministically.
	•	JSON metadata is contract-friendly and marketplace-compatible.
	•	Tier unlockables are created from templates and populated from manifest CIDs.
B. Canonical repository structure (high-level)
	•	01_ASSETS/
	◦	IMAGES/MASTERS (canonical masters, slug named)
	◦	IMAGES/RENDERS/721 (final 3500px or 4096px marketplace images)
	◦	IMAGES/RENDERS/1155 (edition images if needed)
	◦	IMAGES/PREVIEWS, IMAGES/THUMBNAILS, etc.
	•	02_METADATA/
	◦	ERC721/json/<token_id>.json
	◦	ERC721/csv/manifest_from_master.csv
	◦	ERC1155/json/<token_id>.json
	◦	ERC1155/csv/manifest_from_master.csv
	◦	exports/*.csv (tab snapshots)
	•	03_UNLOCKABLES/
	◦	PER_TIER/<Tier>/unlockable_template.txt
	◦	DEITY/<Pantheon>/<Deity>/unlockable_####.txt
	•	04_COLLECTIONS/
	◦	Pantheon/Section/Deity/Collection folder trees
	◦	Each collection folder can contain COLLECTION.md, listing_copy.md, checklist.md, manifest.csv
	•	05_IPFS/ (your existing: CIDs, CARs, etc.)
	•	00_ADMIN/ (license template file, presets, readmes, etc.)

C. Master manifest is the source of truth

What it contains
Tabs (separate sheets) include:
	•	Greek sections: Olympians, Gods & Goddesses, Titans, Creatures, Monsters, Gigantes/Giants, Greek Heroes
	•	Greek: Hero Stories & Collections (Singles rows per story-beat + bundle rows per collection)
	•	Greek Collections (Singles)
	•	Other pantheons: Aztec, Chinese, Egyptian, Hindu, Celtic, Japanese, Norse, Roman
	•	ERC1155 – Editions tab (editions workflow)
New columns added (as requested)
Local asset links (paths):
	•	Masters, Previews, Renders, Thumbnails, Poster Image
CIDs for those assets:
	•	cid_Previews, cid_Renders, cid_Thumbnails, cid_Poster_Image
Commercial/planning fields (not emitted to JSON):
	•	Contract, price_native, currency, chain
License (emitted to JSON as a trait):
	•	license (seeded to 00_ADMIN/LICENSE_TEMPLATE.txt path in workbook; you can replace with public URL)
Naming + trait generation fields:
	•	slug
	•	Title/Name
	•	Pantheon
	•	Meaning/Story
	•	Tier
	•	Stylisation
	•	Frame
	•	Pallette
	•	Format/Medium
	•	token_name (optional override; generated if blank)
Other:
	•	external_url
	•	description
	•	attributes_json (optional hard override; preserves order exactly)
D. Metadata rules (ERC-721 and ERC-1155)
JSON filenames
	•	Always token-id based: <token_id>.json in the respective folder.
Name generation
If token_name is blank:
	•	Auto-build as hyphen-joined: Title/Name-Pantheon-Meaning/Story-Tier-Stylisation-Frame-Pallette-Format/Medium
	•	Skips any blank fields.
Image field logic
	•	Prefer cid_Poster_Image if present; else use image_cid.
	•	If CID is bare and slug exists: ipfs://<CID>/<slug>.jpg
Attributes (ordered, skip blanks)
If attributes_json is provided, use it verbatim (order preserved). Otherwise build attributes in this exact order:
	1	Title/Name
	2	Pantheon
	3	Meaning/Story
	4	Tier
	5	Stylisation
	6	Frame
	7	Pallette
	8	Format/Medium
License in JSON
	•	Emit only License from commercial fields: Append: {"trait_type":"License","value":"<license>"} if license column is filled.
Explicitly not emitted to JSON
	•	Contract, price_native, currency, chain (planning/export only)
E. Automation scripts (current)
v5 generator (license trait + rules)
	•	Sync/Generator Script: sync_json_unlockables_v5.py
This script:
	•	exports per-tab CSVs to 02_METADATA/exports/
	•	writes combined manifests:
	◦	02_METADATA/ERC721/csv/manifest_from_master.csv
	◦	02_METADATA/ERC1155/csv/manifest_from_master.csv
	•	generates:
	◦	02_METADATA/ERC721/json/<token_id>.json
	◦	02_METADATA/ERC1155/json/<token_id>.json
	•	generates unlockables (ERC-721 rows) using per-tier templates:
	◦	03_UNLOCKABLES/DEITY/<Pantheon>/<Deity>/unlockable_####.txt
	•	adds License trait to JSON when provided
	•	does not emit contract/price/currency/chain into JSON
F. Running the script (operator procedure)
Requirements
Python + packages:
	•	pandas
	•	openpyxl
Standard run steps (macOS)
From project root:


python3 -m venv .venv
source .venv/bin/activate
pip install pandas openpyxl

python3 sync_json_unlockables_v5.py --xlsx "GHM_Master_Manifest_PREFILLED_FULL_v4_2025-09-25.xlsx" \
  --fanout --write-json --write-unlockables

Common operator errors encountered and resolved
	•	ModuleNotFoundError: pandas → install deps in venv
	•	“No master manifest xlsx found” → workbook not in root or name mismatch; pass exact --xlsx "<file>"
G. Collections architecture (Greek vs other pantheons)
Greek uses subheadings and story mechanics:
	•	Olympians, Gods & Goddesses, Titans, Creatures, Monsters, Gigantes/Giants, Heroes
	•	Hero Stories:
	◦	(Singles) rows represent “each beat has multiple single NFTs”
	◦	Collection/Chronicle rows represent “bundle purchase (multi-NFT)”
Other pantheons:
	•	Single deity folders, multiple NFTs per deity.
H. Marketplace links and unlockable access
	•	external_url should be a public token page (your site).
	•	Unlockables should be distributed via:
	1	OpenSea Unlockable Content (paste generated unlockable text), and/or
	2	Token-gated unlockables portal (recommended long term)
I. Deliverables produced during the chat (high value artifacts)
	•	Tier unlockables packages (various iterations) culminating in named tiers + correct combos.
	•	Greek collections hierarchy packs (Greek + global pantheons).
	•	FULL collections pack (normalised and non-normalised variants).
	•	Master Manifest workbook(s), culminating in v4 (full + new columns).
	•	Sync scripts:
	◦	v2/v4/v5 (latest recommended: v5)
(If you want, I can generate a single “Release Kit” ZIP that contains the master workbook + v5 script + tier templates + a README so Codex can bootstrap instantly.)

Recommended next steps (operator checklist)
	1	Put the v4 master workbook + v5 script in your project root.
	2	Ensure tier templates exist in 03_UNLOCKABLES/PER_TIER/<Tier>/unlockable_template.txt.
	3	Start with one small subset (e.g., Poseidon 3 tokens) and fill:
	◦	token_id, slug, tier, description, image CID, poster CID if used, license URL, attributes fields
	4	Run v5 and inspect:
	◦	JSONs created correctly
	◦	attributes order correct
	◦	license trait appears
	◦	unlockables generated in correct path
	5	Scale up section by section.
