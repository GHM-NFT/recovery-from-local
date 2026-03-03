GHM NFT Pipeline — Consolidated Summary
Core goal
	•	Build a scalable, cross-platform production + upload system for ~1000 mythology NFTs (multiple cultures) with consistent metadata, naming, and tiering.
Marketplace / minting approach
	•	Use OpenSea as a primary listing platform.
	•	Prefer lazy minting (where possible) to avoid paying mint gas for 1000 items up front; mint happens on purchase (buyer pays mint gas).
	•	You can use OpenSea Studio or your own contract (external/custom) and still list on OpenSea.
Standards & when to use them
	•	ERC-721: for unique pieces / 1-of-1s (and optionally “numbered editions” by minting multiple 721s).
	•	ERC-1155: for true editions (one token with supply N) and for video editions (Relics).
Final 5-tier structure (per deity)
	•	Mythic Icons
	•	Signature Editions
	•	Companion Pieces
	•	Limited Edition Prints
	•	Relics (all videos)
Unlockables (final, per tier)
	•	Mythic Icon: NFT; NFT Video; NFT 10K; VR meta gallery entry; Certificate
	•	Signature Editions: NFT; NFT Video; NFT 8K; Certificate
	•	Companion Pieces: NFT; NFT 6K; Certificate
	•	Limited Edition Prints: NFT; Art Print Token; NFT 10K; Certificate
	•	Relics: NFT; NFT Video; Certificate
Meaning / taxonomy additions
	•	Added structured myth categories to support filtering and clarity:
	•	Mythic Group (e.g., Olympians, Titans, Creatures, Monsters, Giants, etc.)
	•	Being Class (God, Goddess, Hero, Monster, etc.)
	•	Domain (Sea, Wisdom, War, etc.)
	•	Era/Tradition (Classical, Viking Age, etc.)
Poseidon example (your real-world case)
	•	Poseidon has 8 distinct artworks (2 character variants, different frames/colours).
	•	Recommendation: treat each distinct artwork as its own ERC-721 (unique).
	•	Add traits:
	•	Character Variant (e.g., Sea-King / Stormbringer)
	•	Frame Style
	•	Palette / Colourway
	•	Optionally later release some stills as ERC-1155 Limited Edition Prints (separate tokens) and/or derive Relic videos.
Naming system (canonical slug)
	•	Canonical name is set at the Photoshop master stage:
	•	pantheon-deity-title-tier-variant-frame-####.tif
	•	Derivatives append suffixes:
	•	-preview.jpg, -thumb.jpg, -poster.jpg (video posters)
Lightroom vs Photoshop workflow
	•	Masters are created in Photoshop.
	•	You already did colourwork in Lightroom; next step:
	•	LR → Edit a Copy with Lightroom Adjustments → Photoshop
	•	First save in PS = canonical master slug in MASTERS
	•	Then export previews/thumbs from PS
Export sizes (cross-platform safe defaults)
	•	Preview (square): 1600×1600 (2048×2048 for hero pieces)
	•	Thumbnail (square): 512×512 (recommended always for grid consistency)
	•	Video poster: match preview size (e.g., 1600×1600 or 1920×1080)
	•	Video guidance: MP4 H.264 + AAC, 1080p or 4K, loop-friendly
Export presets documentation (added to scaffold)
	•	00_ADMIN/EXPORT_PRESETS_README.md
	•	Quick references:
	•	00_ADMIN/PHOTOSHOP_ACTIONS.txt
	•	00_ADMIN/LIGHTROOM_PRESETS.txt
Folder structure (cross-platform)
	•	Core directories:
	•	01_ASSETS/IMAGES/{MASTERS,PREVIEWS,THUMBNAILS}
	•	01_ASSETS/VIDEO/{MASTERS,PREVIEWS,STILLS}
	•	02_METADATA/{ERC721,ERC1155}/{json,templates,csv}
	•	03_UNLOCKABLES/PER_TIER/... and 03_UNLOCKABLES/DEITY/<Pantheon>/<Deity>/...
	•	05_IPFS/{CAR,CIDs}
	•	06_EXPORT/{OpenSea,Manifold,Rarible,Objkt}
	•	.keep files used so empty folders don’t vanish when zipped.
Metadata templates created
	•	Tier-based JSON templates for:
	•	ERC-721 Mythic Icon
	•	ERC-721 Signature Editions
	•	ERC-721 Companion Pieces
	•	ERC-1155 Limited Edition Prints
	•	ERC-1155 Relics (video with animation_url)
	•	License blurb added into template descriptions (collector display, non-commercial, unlockables referenced).
Per-token unlockable stubs
	•	Sample files like:
	•	03_UNLOCKABLES/DEITY/<Pantheon>/<Deity>/unlockable_0001.txt
	•	Includes placeholders for private links/codes (10K/8K/6K, video, VR entry, COA).
Tier defaults mapping (central control)
	•	00_ADMIN/tier_defaults.json (one place to edit defaults).
	•	Default assumptions (editable):
	•	Mythic Icon: 1, Ethereum, 2.5 ETH
	•	Signature: 25, Polygon, 0.30 ETH
	•	Companion: 12, Polygon, 0.12 ETH
	•	Prints: 50, Polygon, 0.06 ETH
	•	Relics: 100, Polygon, 0.02 ETH
	•	Manifest template CSV includes unlockables_list.
Automation to speed naming/export
	•	Photoshop JSX script (GHM_Filename_Exporter.jsx):
	•	Saves layered master with canonical slug
	•	Exports preview + thumb with suffixes to correct folders
Consolidation / overview tools created
	•	Master Control Sheet (Excel) tabs:
	•	Overview, Tiers, Traits, Folders, Presets, Chain Plan, Deity Planner, Poseidon Pack, Tasks, Tier Defaults
	•	One-page overview markdown summary.
Updates & overwriting
	•	Updates don’t replace folder structure; they add files.
	•	Only potential overwrites are same-name files (notably manifest_template.csv and the Excel if you copy it over).






git remote add origin https://github.com/GHM-NFT/spec.git


# Parent workspace rotate show me
mkdir -p ~/1000 M.2/GitHub Repo/GHM-NFT
cd ~1000 M.2/GitHub Repo/GHM-NFT

# Add your real 9 repo URLs here
repos=(
  "https://github.com/GHM-NFT/spec.git"
  "https://github.com/GHM-NFT/export-Data.git"
  "https://github.com/GHM-NFT/docs.git"
  "https://github.com/GHM-NFT/journey.git"
  "https://github.com/GHM-NFT/prototypes.git"
  "https://github.com/GHM-NFT/Atlas.git"
  "https://github.com/GHM-NFT/config.git"
  "https://github.com/GHM-NFT/Portal.git"
  "https://github.com/GHM-NFT/ghm-nft-portal.git"
)

for url in "${repos[@]}"; do
  name=$(basename "$url" .git)
  if [ -d "$name/.git" ]; then
    echo "Updating $name"
    git -C "$name" fetch --all --prune
    git -C "$name" pull --ff-only
  else
    echo "Cloning $name"
    git clone "$url" "$name"
  fi
done

dowhuz-cYpxe4-nyfzow

# 1) Create parent workspace
mkdir -p "/Volumes/1000 M.2/GitHub Repo/GHM-NFT"
cd "/Volumes/1000 M.2/GitHub Repo/GHM-NFT"

# 2) List all 9 repos here (replace placeholders with your real URLs)
repos=(
  "https://github.com/GHM-NFT/spec.git"
  "https://github.com/GHM-NFT/export-Data.git"
  "https://github.com/GHM-NFT/docs.git"
  "https://github.com/GHM-NFT/journey.git"
  "https://github.com/GHM-NFT/prototypes.git"
  "https://github.com/GHM-NFT/Atlas.git"
  "https://github.com/GHM-NFT/config.git"
  "https://github.com/GHM-NFT/Portal.git"
  "https://github.com/GHM-NFT/ghm-nft-portal.git"
)

# 3) Clone if missing, otherwise update
for url in "${repos[@]}"; do
  name=$(basename "$url" .git)
  if [ -d "$name/.git" ]; then
    echo "Updating $name..."
    git -C "$name" fetch --all --prune
    git -C "$name" pull --ff-only
  else
    echo "Cloning $name..."
    git clone "$url" "$name"
  fi
done

# 4) Verify what you have
pwd
ls -la
for d in */.git; do
  repo="${d%/.git}"
  echo "=== $repo ==="
  git -C "$repo" remote -v
done
