1. Using The test script sheet to test scripts for # Google Sheets → Google Docs System Handover (Export-Ready Running Order): Testing Scripts Test Sheet - GHM Olympians - 4/3/26

1a	The goal as this stage is have a clean google doc that can export cleanly all the data or specified data. Then to rebuild the local 	folder system with all the correct files in the correct folders.
	
	Secondly to export the data required for the website GHM, specifically for the Atlas system (Discused in docs/). This will allow the 	completion of the GHM website with the correct data.

	Thirdly to export the data required to upload to Opensea for the NFT to be sold.

So we were working on this to export the correct data for the website (Atlas system):
First 10 items: file types + sizes to export

- `00_sheet_exports/` -> `06_GHM_NFT_Project/02_METADATA/exports/`
- `01_sources/` -> `06_GHM_NFT_Project/01_ASSETS/`
- `02_renders/images/` -> `06_GHM_NFT_Project/01_ASSETS/IMAGES/PREVIEWS/` + `.../THUMBNAILS/`
- `02_renders/video/` -> `06_GHM_NFT_Project/01_ASSETS/VIDEO/`
- `03_metadata/json/` -> `06_GHM_NFT_Project/02_METADATA/`
- `05_delivery/upload_ready/` -> `06_GHM_NFT_Project/06_EXPORT/<Marketplace>/`


	•	2) First 10 items: file types + sizes to export
	•	•	Preview image: JPG, sRGB, quality 80–85, usually 1600×1600 (or 2048×2048 for hero detail). 
	•	Thumbnail: JPG, sRGB, 512×512 (always square thumb).  Size targets:
	•	Preview < 1 MB
	•	Thumb < 120 KB  
If a token is video-based, also export:
	•	Poster JPG (for image) matching preview size, plus
	•	MP4 master (H.264 + AAC, 1080p30 or 4K30).


3) Correct filename when exporting
Use slug-based naming:13	§1
	•	<slug>-preview.jpg
	•	<slug>-thumb.jpg
	•	(video) <slug>-poster.jpg
	•	(video master) <slug>-4k30-h264-aac-v1.mp4 (or your 1080p naming variant) -1080p-h264-aac-v1.mp4


4) Photoshop metadata — what should you add?
For marketplace performance, do minimal metadata:
	•	Keep sRGB embedded profile
	•	Strip heavy EXIF / unnecessary metadata
	•	Optional: keep copyright/contact only if needed
That is exactly the current standard in your preset spec.

So in Photoshop export terms:
	•	Convert to sRGB
	•	Export JPG quality 80–85
	•	Minimize metadata payload


Title (exact artwork title)
Creator / Artist (your name / studio name)
Copyright Notice (e.g., © 2026 Mark Preston / Moda Digital Ltd.)
Rights Usage Terms (short usage statement)
Description (short artwork description)
Keywords (collection, character, pantheon, medium, edition type)
Creation Date
Contact / Website URL (project or artist site)
Project / Series (e.g., collection name)


1b. These 2 are the main scripts that you worked on before and were working well.
ghm_single_kit_debugged.gs - the main script with many functions
/*************************************************
 * GHM — SINGLE KIT (debugged hardening pass)
 * Notes:
 * - Keeps one onOpen menu entrypoint.
 * - Uses bulk reads for compact views (faster, fewer API calls).
 * - Handles header aliases and safer control autofill.
 **************************************************/
The drop down scripts menu contains:
GHM_NORM
GHM_headerMap
GHM_bodyRows
GHM_ensureCol
GHM_firstCol
BuildAllMenusNow
onOpen
Control_Setup
Control_Apply_Safe
Run_Derivations_IfAvailable
Fix_Sanitize_TextColumns
Fix_Standard_Contract_All
Fix_DedupeHeader_ContractFactory
Control_Rebuild_ Report
GHM_roleMap
Color_Repaint_ThisTab
View_Compact_Active_HARD
View_Compact_All_HARD
View_Compact_Cleanup


Stage_One_Sandbox_debugged.gs
/***************************************
 * GHM — Stage_One_Sandbox (debugged)
 * Focused fixes:
 * - single onOpen
 * - no undefined preview menu items
 * - no duplicate control enforcement in orchestrators
 * - bulk media URL reads (performance)
 * - safe delete order for legacy preview cols
 ***************************************/
The drop down scripts menu contains:
RUN _update All _now
GHM_RemoveLegacyPreviewColsOnce


helpers_restore.gs - has lots of functions
/** Helpers restore bundle
 * Drop into Apps Script as a single file. Implements:
 * - backupSpreadsheet
 * - fixCommonNamesGeneric
 * - ensureHeaderAliases
 * - populateTitleDisplayAndShort_Generic
 * - forceOverwriteTitleEn_WithProtectionHandling_Generic
 * - forceRewriteAltTextEn_Targeted_Generic
 * - applyDataValidationRules_Generic
 * - addHiddenOpsFlagAndHideCols_Generic
 * - runSmokeTest_Generic
 * - runRefreshAutosSandbox
 * - runRefreshAutosForSheet
 *
 * Safe defaults: operate on sheetName argument; if omitted use active sheet.
 */
The drop down scripts menu contains:
backupSpreadsheet
fixCommonNamesGeneric
ensureHeaderAliases
populate TitleDisplay
AndShort_Generic
forceOverwriteTitleEn_WithProtectionHandlin.
forceRewriteAltTextEn_Targeted_Generic
applyDataValidationRules_Generic
addHiddenOpsFlagAndHideCols_Generic
runSmokeTest_Generic
runRefreshAutosSandbox
runRefreshAutosForSheet
columnToLetter


1c. The atlas system all needs a traits/stamp system (12 options) that designates each NFT with one of the 12 traits/stamp states.

1d. The Schema_Freeze is very out of date and needs updating to latest version; Test Sheet - GHM Olympians. Don't know how to do that.

1e. At the moment I am unsure how to update the main manifest from the GHM_SoT Tab, very important it works?
The GHM_SoT has a number of columns that update the collection doc (Manifest_GHM_Olympians). The desired columns which should feed are:

Character_Variant	myth_scene	Style	Colourway	Frame	slug(Not this one but the system said it was required for it to work)	meaning_line	caption_300	symbols	taxonomy_tags	caption_long_en	research_notes	sources_bibliography	sot_link

Then there are lots of addon columns that were added when being used which are not needed:
standard	contract_factory	Contract address	operator_filter	operator_policy_note	license_url	royalty_bps	fee_recipient	reveal_mode	reveal_date	freeze_policy	freeze_date	primary_mint_platform	est_gas_mint	HIDDEN_OPS - columns hidden	

2. The menus (GHM Autos and GHM) are now missing once I added the language script? This script GHM_Languages.gs

3. I dont need any manipulation of tab colours or compacting manipulation of the columns for viewing, i will handle all colours, delete anything related to colouring and compacting tabs.

4. I dont want to have multiple collections on one sheet, each collection will have its own sheet, so for example GHM_Master_Manifest will be divided up into collections, so for example GHM_Master_Manifest_The_Olympians, GHM_Master_Manifest_The_Titans. etc. This simplifys things. So advice which scripts are redundent and which scripts need updating?

5. Fill_Taxonomy_From_Mapping.gs - Not Working error says: Taxonomy_Mapping needs headers: Series, Character, Character_Variant, category_id (and optionally subcategory_id, taxonomy_tags).

On the Taxonomy_mapping sheet there is only series, category_id, subcategory_id

6. GHM_Canonicalize.gs - Showing an error when initiating Apply to all collection tabs. I don't think I need this anymore as I plan to have one sheet per series now. Is that correct?
GHM_CANON_Apply_To_All
Exception: Service error: Spreadsheets
(anonymous)	@ GHM_Canonicalize.gs:77
GHM_CANON_Apply_To_AllTabs	@ GHM_Canonicalize.gs:46

7. Can you help with the 3 language scripts:
GHM_Languages.gs
GHM_LANG_Prefill_JaZhHiIt.gs
GHM_Languages_L3_FixKit.gs

These scripts are only relevant for the language changes for the following collections:
Chinese (All language columns added on collection sheet)
Hindu (All to be added)
Japanese (All to be added)
Roman (All to be added)

For example the chinese collection has scripts either in the columns or in the scripts that create a chinese language. They have extra columns:
title_zh-Hans
description_zh-Hans
alt_text_zh-Hans
fillAltTextEn_(sh)
fillAltTextZH_(sh)

These are used to give both an english and chinese languages in the export and then used in a way ive forgotten, Can you help?

9. ghm_tokenid_suffix_columns.md - Working, append token_id suffix to selected derived columns.

10. Metadata_Refresh.gs - this is working but with one error it is not updating the file name extension, which should be covered by 
 * - Recomputes meta_title_auto, print_eligible_inferred, alt_text_en_auto,
 *   hook_line, attributes_json, and ensures media_filename has an extension.
 * - Safe: creates a sheet backup and writes QA_Report_Derived.

12. Active_QA.gs - This is working and created a Active_QA_Report with the following header and data:
sheet	checked_at	check	status	issues	notes / first few rows
Manifest_GHM_Olympians	04/03/2026 06:13:34	token_range format			OK	0	
Manifest_GHM_Olympians	04/03/2026 06:13:34	attributes_json looks like traits	OK	0	
Manifest_GHM_Olympians	04/03/2026 06:13:35	Tier allowed values			OK	0	
Manifest_GHM_Olympians	04/03/2026 06:13:35	background_hex valid			OK	0	
Manifest_GHM_Olympians	04/03/2026 06:13:36	edition_size integer > 0		OK	0	
Manifest_GHM_Olympians	04/03/2026 06:13:37	Style is descriptive (not version)	OK	0	
Manifest_GHM_Olympians	04/03/2026 06:13:38	Series ≠ Pantheon			OK	0	
Manifest_GHM_Olympians	04/03/2026 06:13:38	image CID looks valid			OK	0	
Manifest_GHM_Olympians	04/03/2026 06:13:39	json_filename ends .json		OK	0	
Manifest_GHM_Olympians	04/03/2026 06:13:40	slug format (a-z0-9-)			OK	0	

13. dev_tools.gs - This is working and checks expected functions and created a new tab report called QA_Report_Functions:
function	status
backupSpreadsheet			present
fixCommonNamesGeneric			present
ensureHeaderAliases			present
populateTitleDisplayAndShort_Generic	present
forceOverwriteTitleEn_WithProtectionHandling_Generic	present
forceRewriteAltTextEn_Targeted_Generic	present
applyDataValidationRules_Generic	present
addHiddenOpsFlagAndHideCols_Generic	present
runSmokeTest_Generic			present
runRefreshAutosSandbox			present
runRefreshAutosForSheet			present
refreshActiveTab			present
onOpen					present
checkExpectedFunctions			present

14. One script function create a QA_Report which is incorrect when looking?

15. GHM_Audit_Final.gs - Don't know what is does?

16. Contact_Address_Update.md  - Don't know what is does?

17. X_Fix_active_tab.md - Is this needed anymore?
/ One-tab fixer: adds schema_version, moves "v1.x" out of Style, merges S -> Series.
// ES5-safe, no external helpers needed.

18. Refresh_Active_Tab.md - On the script and in the menu: Creates a number of QA's and makes a bit of a mess with backups with dates on them  and one copy of...(created 4?), created a QA_Report_TitleForce with following data:
row	new_title_en	status
2	Zeus — The Wise (Test me now you know Im free)			ok
3	Poseidon — The Sea Master (Test me now you know Im free)	ok
4	Poseidon Two							ok
5	Poseidon — The Sea Master (Test me now you know Im free)	ok
6	Ares — The King Of War (Test me now you know Im free)		ok
7	Hades — King Of The Underworld (Test me now you know Im)	ok
8	Aphrodite — The Queen Of Love (Test me now you know Im free)	ok
9	Apollo — The Poet & Artist (Test me now you know Im free)	ok
10	Athena — Goddess Of War (Test me now you know Im free)		ok
11	Hera — Test Character_Variant. TEST. (Test me now you know)	ok
12	Hermes — Test Character_Variant. TEST. (Test me now you)	ok
13	Artemis — Test Character_Variant. TEST. (Test me now you)	ok
14	Hephaestus — Test Character_Variant. TEST. (Test me now you)	ok
15	Hestia — Test Character_Variant. TEST. (Test me now you)	ok

Also created a QA_Report but with no data shown?;
row	issue	field	value	notes

19. GHM_FastCompact.md - don't know if this is needed anymore?

20. This is the Atlas Concept and handover:
spec + acceptance criteria

# GHM Atlas — Concept + Production Plan + Codex Handover (v1)

## 1) What Atlas is (one-paragraph intro)
**Atlas** is the logged‑in collector hub for *Gods • Heroes • Myths (GHM)*. It turns individual NFTs (stamps, teasers, editions) into **progress on story journeys** (3/4/6/12 beats). When a collector completes a recipe (e.g., 12/12), Atlas unlocks a **Chronicle Claim**: a minted “Chronicle” badge + a generated **Chronicle Poster** (digital, and optionally a physical print redemption for full journeys).

---

## 2) The core idea in bullets (for first‑time readers)

### Story-first collecting
- Collectors don’t buy “random items”; they **collect a narrative**.
- Every story is broken into numbered **beats** aligned with the **12-stage Hero’s Journey** (or shortened 3/4/6 recipes).
- Each beat has **visual UI** (block tile / stamp) and **meaning** (title + short description).

### Two NFT types (simple mental model)
- **Stamp** = the repeatable, edition-style collectible that fills a beat (often affordable).
- **Teaser (1/1)** = premium spotlight artwork tied to a beat (optional crown status). It can also count for the beat if configured.

### Recipes (3 / 4 / 6 / 12)
- Stories can be offered in different lengths without changing the logic:
  - **3-beat**: mini arc
  - **4-beat**: short quest
  - **6-beat**: chapter set
  - **12-beat**: full hero’s journey / epic
- UI layout adapts (grid / shrine / cascade) but the **beat numbers remain canonical**.

### Completion & rewards
- Completion does **not auto-ship** anything.
- Completion unlocks the **right to claim**:
  - Always: Chronicle NFT + generated digital Chronicle poster
  - Optional (policy): physical print redemption for 12-beat completion

---

## 3) “How it works” — the mechanics

### Beat ownership
A wallet “owns” a beat if any of the following is true (configurable):
- The wallet owns a **Stamp NFT** mapped to that story+stage
- The wallet owns a **Teaser 1/1** mapped to that story+stage
- (Optional) the wallet owns an approved **bundle / pack** token that grants the beat

### Progress calculation
For a story:
- `ownedStages = set(stages from owned NFTs)`
- `progress = ownedStages.size / totalStagesInRecipe`
- Recipe complete when `ownedStages.size >= requiredStagesCount`

### Chronicle claim
When complete:
1. User clicks **Claim Chronicle**
2. Atlas checks eligibility (on-chain ownership + not already claimed for that story/recipe)
3. Mints a **Chronicle** (or records claim off-chain if you prefer)
4. Generates:
   - Poster preview PNG for web
   - Print-ready PDF (template-driven)
5. Optionally issues a **Print Voucher** (redeem once)

---

## 4) The six logged‑in Atlas pages (user-facing admin hub)

### A) Atlas Hub (Library)
Purpose: browse stories and spotlights, and resume progress.
Key modules:
- Search + filters (culture, recipe size, status)
- “Your Journeys” summary (3/4/6/12 counts)
- Stories grid cards (progress bar + continue)
- Spotlights grid (single gods/creatures)

### B) Story Page (Journey)
Purpose: see the beat grid and pick a beat.
Key modules:
- 12-block (or recipe layout) grid with states: locked / hover / owned / teaserOwned
- Stage detail panel: title, description, owned items (stamp/teaser), marketplace links
- Filter: All / Owned / Locked

### C) Teaser Page (1/1)
Purpose: premium purchase page focused on artwork.
Key modules:
- Large artwork media
- Buy panel + license link
- Story linkage: completes Stage N
- Provenance traits + QR

### D) Stamp Detail
Purpose: edition collectible page.
Key modules:
- Stamp hero (scene/relic/type)
- Edition info (supply, price, status)
- “Completes Stage N” chip
- Related stamps for same beat (other styles)

### E) Chronicle Claim
Purpose: prove completion and claim rewards.
Key modules:
- Recipe preview filled with owned beats
- Eligibility status + missing list
- Claim button
- After claim: badge + downloads (PNG/PDF) + optional print redemption CTA

### F) Profile
Purpose: collector dashboard.
Key modules:
- Wallet header (ENS/address, network)
- Tabs: Journeys / NFTs / Chronicles
- Journeys: story cards + progress
- NFTs: filterable grid (story, stage, style)
- Chronicles: claimed list + downloads

---

## 5) Production plan (practical, step-by-step)

### Phase 0 — Decisions (1–2 hours)
- Confirm the “beat ownership rules” (stamps, teasers, bundles?)
- Confirm claim policy for prints (12-beat only? shipping charged?)
- Confirm canonical recipes: 3/4/6/12 only (recommended)

### Phase 1 — Data model & mapping (1 day)
Create a single source of truth (Google Sheet → JSON export) with:
- `story_id`, `story_title`, `culture`, `recipe_default`
- beats: `stage_number`, `stage_label`, `beat_title`, `short_desc`, `icon_key`
- NFT mappings:
  - `token_contract`, `token_id` OR `collection_slug`
  - `type` = stamp|teaser
  - `maps_to_story_id`, `maps_to_stage_number`

### Phase 2 — UI build (2–4 days)
- Build the six pages as a responsive app (iframe-ready)
- Implement states and transitions:
  - locked vs owned vs teaserOwned
- Implement filters and tabs

### Phase 3 — Wallet & security (2–4 days)
- Wallet connect (WalletConnect + ethers/viem)
- Read ownership via RPC (Alchemy/Infura) + contract ABI (primary)
- Session: Sign-in with Ethereum (SIWE) recommended

### Phase 4 — Chronicle generation (2–5 days)
- Template-based poster generation (PNG/PDF)
- Store in S3/R2 or IPFS, return download links
- Claim tracking (DB table or contract mint)

### Phase 5 — QA + launch readiness (1–2 days)
- Test recipe completion edge cases
- Mobile tap targets, performance, caching
- Verify license_url + disclosure sections

---

## 6) Codex handover — what to build, clearly

### Deliverables (definition of done)
1. Iframe-ready Atlas app with six routes:
   - `/atlas` (hub)
   - `/atlas/story/:storyId`
   - `/atlas/teaser/:slug`
   - `/atlas/stamp/:slug`
   - `/atlas/claim/:storyId`
   - `/atlas/profile`
2. Wallet auth (SIWE) + session
3. Ownership resolver:
   - Input: wallet address
   - Output: list of owned mapped beats
4. Recipe completion logic + claim gating
5. Poster generation endpoint:
   - Input: storyId + recipe + owned beat assets
   - Output: preview PNG + PDF URLs

### Suggested stack (works well for Codex)
- Next.js (App Router) + TypeScript
- Tailwind CSS (match black/gold theme)
- Ethers.js or viem for on-chain reads
- SIWE for login
- Storage: S3/R2 + signed URLs
- DB: Postgres/SQLite (claims, users, downloads)

### Repo structure (simple)
- `apps/atlas` — Next.js app (iframe embed)
- `packages/ui` — shared components
- `packages/data` — story config loader + types
- `packages/chain` — ABIs + ownership resolver
- `services/poster` — poster renderer (server-side)

### API endpoints (minimal)
- `GET /api/config/stories` → story list + metadata
- `GET /api/config/story/:storyId` → beats + mappings
- `POST /api/auth/siwe` → session
- `GET /api/owned?wallet=0x…` → owned beats list
- `POST /api/claim` → claim chronicle (idempotent)
- `POST /api/poster` → generate poster (returns URLs)

### Key edge cases
- User owns multiple items for the same beat (show both in detail panel)
- Teaser counts for beat only if mapping says so
- Claim can be done once per story+recipe (enforce server-side)
- Network mismatches (ETH vs Base) — clear UI

---

## 7) Conclusions (the “why this works” summary)
- Atlas turns your art into **collectible narrative progress**.
- Stamps keep it scalable; Teasers keep it premium.
- Recipes let you launch fast with 3/4/6 while still supporting epics (12).
- Chronicle claims create a powerful “completion moment” and a tangible artifact (poster/print).

---

## 8) What you (M) need to provide to start dev
- Final UI designs in Elementor OR screenshots per page section
- Asset bundle: story covers, beat stamps (owned/locked/minis), 12 universal icons
- Story config sheet export (CSV/JSON) for the first 3 stories

