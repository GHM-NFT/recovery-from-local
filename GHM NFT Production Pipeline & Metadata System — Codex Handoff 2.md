1) First-time Reader Summary
What you’re building
	•	A mythology NFT ecosystem (~1000 items) across multiple cultures.
	•	A repeatable production pipeline so you can prepare assets + metadata once, then upload one-by-one or in batches to OpenSea and other platforms.
The five tiers (per deity)
	1	Mythic Icons (top tier)
	2	Signature Editions
	3	Companion Pieces
	4	Limited Edition Prints
	5	Relics (all videos)
What collectors receive (unlockables)
	•	Mythic Icon: NFT, NFT Video, NFT 10K, VR meta gallery entry, Certificate
	•	Signature Editions: NFT, NFT Video, NFT 8K, Certificate
	•	Companion Pieces: NFT, NFT 6K, Certificate
	•	Limited Edition Prints: NFT, Art Print Token, NFT 10K, Certificate
	•	Relics: NFT, NFT Video, Certificate
How you organize files (so everything stays aligned)
	•	You use one root folder (GHM_NFT_Project) with sections for:
	◦	Masters, Previews, Thumbnails
	◦	Video masters, video previews, video posters
	◦	Metadata JSON (ERC-721 and ERC-1155)
	◦	Unlockable text files
	◦	IPFS CID logs
	◦	Exports per marketplace
The key rule: naming
	•	The Photoshop master filename is the “source of truth.”
	•	Lightroom should not rename anything.
	•	Derivatives only add suffixes (-preview, -thumb, -poster).
Image/video sizes (safe across marketplaces)
	•	Preview (square): 1600×1600 (or 2048×2048 for hero pieces)
	•	Thumbnail (square): 512×512
	•	Video poster: match preview size
	•	Relic video: MP4 H.264 + AAC (or silent), 1080p/4K, loop-friendly
Your Poseidon example
	•	Poseidon has 8 unique artworks (2 character variants + different frames/colours).
	•	Each distinct artwork should be treated as its own unique token (ERC-721).
	•	Add traits for:
	◦	Character Variant
	◦	Frame Style
	◦	Palette/Colourway
Tools created to reduce confusion
	•	A Master Control Sheet (Excel) with tabs for Overview, Tiers, Traits, Presets, Tasks, and a Poseidon planner.
	•	A Photoshop JSX script to automate naming + exporting preview/thumb in the correct folders.
	•	README docs describing export presets and actions/presets for Lightroom/Photoshop.


Objective
Implement and maintain a consistent, scalable NFT asset+metadata pipeline for the Gods Heroes Myths collection (~1000 tokens), supporting:
	•	OpenSea primary listing
	•	Cross-marketplace compatibility (Rarible/Manifold/Objkt etc.)
	•	Tiered product strategy (5 tiers per deity)
	•	Repeatable file naming, export presets, unlockables, and IPFS CID tracking
Canonical Decisions
Token Standards
	•	ERC-721: unique works (Mythic Icons, Signature Editions, Companion Pieces)
	•	ERC-1155: identical editions (Limited Edition Prints) and video editions (Relics)
Five Tiers (per deity)
	1	Mythic Icons
	2	Signature Editions
	3	Companion Pieces
	4	Limited Edition Prints
	5	Relics (video)
Unlockables Mapping (tier → items)
	•	Mythic Icons: NFT; NFT Video; NFT 10K; VR Meta Gallery entry; Certificate
	•	Signature Editions: NFT; NFT Video; NFT 8K; Certificate
	•	Companion Pieces: NFT; NFT 6K; Certificate
	•	Limited Edition Prints: NFT; Art Print Token; NFT 10K; Certificate
	•	Relics: NFT; NFT Video; Certificate
Metadata Schema (traits and ordering)
Order: Pantheon → Deity/Myth → Mythic Group → Being Class → Domain → Tier → Medium → Format → Palette → Symbol → Background Motif → Era/Tradition → Year
Additional operational traits:
	•	Character Variant (e.g., Sea-King / Stormbringer)
	•	Frame Style (e.g., Bronze Round, Coral Filigree)
Video Metadata Rule (Relics)
	•	Metadata uses:
	◦	image = poster still (JPG)
	◦	animation_url = MP4 file
File System Specification
Root
GHM_NFT_Project/
Key directories
	•	00_ADMIN/
	◦	export preset docs, actions/presets, tier defaults
	•	01_ASSETS/IMAGES/MASTERS/ (Photoshop masters; canonical slug)
	•	01_ASSETS/IMAGES/PREVIEWS/ (JPG web previews)
	•	01_ASSETS/IMAGES/THUMBNAILS/ (JPG thumbs)
	•	01_ASSETS/VIDEO/MASTERS/ (MP4 relic masters)
	•	01_ASSETS/VIDEO/PREVIEWS/ (lighter MP4 previews)
	•	01_ASSETS/VIDEO/STILLS/ (poster images for videos)
	•	02_METADATA/ERC721/{templates,json,csv}/
	•	02_METADATA/ERC1155/{templates,json,csv}/
	•	03_UNLOCKABLES/PER_TIER/<tier>/
	•	03_UNLOCKABLES/DEITY/<Pantheon>/<Deity>/unlockable_####.txt
	•	05_IPFS/CIDs/ (CID logs for images/video/json)
	•	06_EXPORT/<Marketplace>/ (ready-to-upload bundles)
Empty-folder persistence
	•	.keep files used so ZIP extraction retains empty folders.
Naming Spec (Canonical Slug)
Master naming (Photoshop)
Canonical slug set at first PS master save: pantheon-deity-title-tier-variant-frame-####.tif
Derivative naming (suffixes only)
	•	Preview: ...-preview.jpg
	•	Thumb: ...-thumb.jpg
	•	Video poster: ...-poster.jpg
	•	Video master includes tech suffixes (optional but recommended): ...-4k30-h264-aac-v1.mp4
Export Presets (Implementation Targets)
	•	Preview (square): 1600×1600 (hero: 2048×2048)
	•	Thumbnail (square): 512×512
	•	Poster for video: match preview size
	•	Video: MP4 H.264 + AAC (or silent), 1080p or 4K, loop-friendly
Pipeline (Operational Steps)
	1	Photoshop master created → save into MASTERS/ using canonical slug.
	2	Lightroom colour grading (no renaming).
	3	Back to PS for finishing → export preview/thumb/poster using suffixes to correct folders.
	4	Create/duplicate JSON from tier templates → fill traits and later insert IPFS URIs.
	5	Unlockable text created per token → paste links/codes.
	6	Pin assets + metadata to IPFS → record CIDs in 05_IPFS/CIDs/.
	7	Update JSON image/animation_url to ipfs://…
	8	Upload/list via marketplace; freeze metadata where applicable.
Artifacts Delivered (to be used by Codex)
	•	Project scaffolds (ZIPs) including folder structure, templates, unlockables, and presets:
	◦	GHM_NFT_Project_with_Export_Presets_and_Actions.zip
	◦	GHM_NFT_Project_5_Tiers_With_Unlockables_and_Stubs.zip
	•	Photoshop automation script:
	◦	GHM_Filename_Exporter.jsx
	•	Master Control Sheet (Excel):
	◦	GHM_Master_Control_Sheet.xlsx
	•	Single-page overview:
	◦	GHM_Master_Overview.md
	•	Tier defaults mapping update:
	◦	GHM_Tier_Mapping_Update.zip
	•	Canvas:
	◦	“GHM NFT Pipeline — Consolidated Summary” (acts as living spec)
Poseidon Test Case (for validation)
	•	8 unique Poseidon artworks (2 character variants, multiple frames/colourways).
	•	Expected: 8 ERC-721 JSONs, each with Character Variant + Frame Style traits.
	•	Optional: choose select artworks for later ERC-1155 Limited Edition Print releases and/or Relic videoderivatives.
Codex Implementation Notes
	•	Treat the Excel “Master Control Sheet” as the human-facing spec.
	•	Treat 00_ADMIN/tier_defaults.json as the machine-facing defaults (good anchor for any scripts).
	•	Any future automation should:
	◦	derive filenames and folder paths from the canonical slug
	◦	generate derivative filenames by suffix rules
	◦	generate JSON fields from tier templates + trait dictionary
	◦	keep OpenSea-compatible JSON (name, description, image, animation_url, attributes[])
