nft-collection-columns-master-spec

Column Autos & Dynamic Content — merged spec (v2025‑10‑17)
Basis: GHM Profile (colors & conventions), prior Column‑Autos list, and two reference memos you flagged. This doc merges them into one implementable spec.

0) Color legend & general rules
	•	RED = manual/curation (human judgement; keep short; avoid giant formulas).
	•	BLUE = setup/ops (addresses, prices, standards, platform choices).
	•	YELLOW = auto‑if‑available (pulled from SoT or filled by downstream tools after upload).
	•	GREEN = computed/derived (via RUN_updateAll_now script; header‑safe; minimal in‑cell formulas).
	•	Do not auto‑repaint colors (per GHM profile). Colors set once by you.
	•	All autos are header‑safe: the script finds columns by header text (not by letter).

1) Manifest — GREEN autos (computed)
Purpose: output clean metadata, QA flags, and operator‑safe exports.
	1	slug → unique, lowercase hyphen slug.
	◦	Uses: Series, Character, Character_Variant, myth_scene, Style. If duplicate across tab, append -<token_id>. 
	2	hook_line → short one‑liner for cards.
	◦	Uses: name_final, Character, Character_Variant, Style, Tier, myth_scene.
	3	alt_text_en_auto, alt_text_zh‑Hans_auto (if zh planned) → metadata‑based alt text.
	◦	Uses: Character, Character_Variant, Style, Frame, Colorway, meaning_line, Tier. (See §7 templates.) Whats this templates mean?
	4	attributes_json → traits array as JSON (stable order):
	◦	Order: Pantheon/Culture → Series/Deity Collection → Character → Variant/Location → Myth Scene → Style/Format → Tier → Edition Size → Symbols (multi) → Colorway → Frame.
	5	category_id, subcategory_id → taxonomy lookup from a Map tab.  Can you fill out spome of these to give me help, look at the work done on NFT Strategy
	6	image_url, animation_url → IPFS gateway URLs built from image_cid / animation_cid.
	7	preview - manual upload
	8	media_filename → export stem = slug + correct file extension from image_mime.
	9	json_filename → slug + ".json".
	10	external_url_built → site token URL = BASE_SITE + slug.
	11	edition_string → human display (e.g., Edition of 10).
	12	json_will_emit → TRUE/FALSE gate (all required fields present & valid).
	13	missing_fields / warnings → human‑readable issues (missing RED/BLUE, too‑long copy, bad hex, mime/bytes missing, etc.).
	14	len_* + status_* → length counters & traffic‑light status for: name_final, Series, Character, Character_Variant, myth_scene, Style, Tier, edition_size, slug, description_en, caption_300. (See §6 budgets.)Whats do these budgets mean?
	15	checksum_ok (optional) → flags empty bytes or hash mismatch when available.
	16	sot_status → TRUE/FALSE if slug found on SoT tab.
	17	print_eligible_inferred (optional) → suggest Y/N from Tier & media presence (does not override RED flag). Whats does thiss mean?
	18	provenance_block_sha (optional, per‑collection) → SHA‑256 commitment of ordered media list (see §8.1).  Whats does thiss mean?

2) Manifest — YELLOW autos (pulled/filled later)
	•	caption_long_en, research_notes, sources_bibliography → XLOOKUP by slug from SoT.  Whats does thiss mean?
	•	opensea_permalink, json_preview_url → written post‑upload. Whatsabout other platforms being used?
	•	json_path, json_cid → written by packager/export.
	•	unlockable_zip_filename / url / bytes / sha256 → written by packager.
	•	thumb_url → if you store a CDN/thumb externally for your site. Whats this used for, example of the NFT, what size is this if it is a preview?

3) Manifest — BLUE (setup/ops; keep)
	•	license_url, license_attr_value  Can you explain this to me please, what do i input?
	•	price_native, currency
	•	royalty_bps, fee_recipient (or royalty_recipient)  Can you explain this to me please, what do i input?
	•	operator_filter (on/off)
	•	chain (ETH / Base / Polygon)
	•	contract_address, contract_factory, standard (721 / 1155)
	•	image_filename, animation_filename  - What do I name these files?
	•	cid_Poster_Image (if you keep) and image_cid, animation_cid
	•	external_url (collection landing) Is this different for every NFT, is this to what website?
	•	token_range (if editions) & optional collection_path  - Can you explain this to me please, what do i input?
	•	reveal_mode (Instant / Delayed), reveal_date, pre_reveal_cid (if delayed)
	•	edition_label (e.g., "Signature 8")
	•	burnable, redeem_target, redemption_window, claims_remaining (if using redeem flows)  Can you explain this to me please, what do i input?
	•	print_eligible (manual flag)
	•	physical_redeemable (manual flag)
	•	background_hex (tile color) — keep #RRGGBB format

4) SoT (Source‑of‑Truth) tab
Create GHM_SoT with one row per work, keyed by slug.  Is this a new tab on the google sheet, Should this not various columns automatically generated from the collection tabs?
Columns:
	•	slug Generated from collections tabs?
	•	caption_long_en. Generated from collections tabs?
	•	research_notes.  Can you explain this to me please, what do i input?  Generated from collections tabs?
	•	sources_bibliography.  Can you explain this to me please, what do i input?
	•	(optional) sot_doc_url (Google Doc or Notion). Can you explain this to me please, what do i input?
	•	(optional) sources_raw (scratch), draft_status (enum)  Can you explain this to me please, what do i input?
In Manifest (Yellow / Green):
	•	caption_long_en (YELLOW) ← lookup by slug
	•	research_notes (YELLOW) ← lookup by slug
	•	sources_bibliography (YELLOW) ← lookup by slug
	•	sot_status (GREEN) = found/missing
	•	sot_link (BLUE) = hyperlink to sot_doc_url (optional)Can you explain this to me please, what do i input?

5) Control sheet — ops & policy columns - Can you explain this to me please, which of this is new? Which of this is blue, yellow,green or red? Are these flexible enough to cover all the key decisions and the tiers and the differences in platforms, timings etc. Can you explain what i input into each one please?
Keep policy & platform choices centralized:
	•	standard (721/1155), edition_size, token_range
	•	royalty_bps, royalty_recipient
	•	chain, est_gas_mint, est_gas_batch
	•	operator_filter (on/off) + short policy note
	•	freeze_policy, freeze_date
	•	contract_factory (Manifold/Zora/Custom), collection_address, collection_slug
	•	allowlist_source, claim_start, claim_end, per_wallet_cap, reserve_supply
	•	locale_default (e.g., en-GB) & Planned_Languages tab (only add languages you plan)
	•	Marketplace/profile: primary_mint_platform, marketplace_targets, verification_status
	•	Web/SEO: meta_title, meta_description, og_image, canonical_domain_or_ENS
	•	Legal: license_type, terms_url, physical_terms_url, moral_rights_statement
	•	Pricing/release ops: target_fiat_price, dynamic_pricing, batch_release_plan_url, press_kit_url, support_email

6) Length budgets & validators (GREEN)
	•	Display name (name_final) target ≤ 60 chars (AMBER up to 80; RED above 80).
	•	Trait values target ≤ 24 chars (AMBER ≤30; RED >30).
	•	Slug/media/json filename: GREEN ≤100, AMBER ≤120, RED >120.
	•	JSON gate fails if: missing image_cid, bad background_hex, empty license_url, or length in RED.

7) Alt‑text templates (GREEN; zh only if planned) Can you explain this to me please,?
English template (filled by script):
"Round canvas portrait of {Character}{Variant}, {Style}{Frame?}, {Colorway} palette; {meaning_line}. {Tier} edition." - Where is the column which is adding Round Canvas Portrait, I must be able to change this. option?
Examples:
	•	"Round canvas portrait of Poseidon (Variant B), painted Style with ornate Aegean frame, storm‑teal palette; god of the sea. Signature edition."
Chinese (zh‑Hans) template (machine‑translated, then human‑reviewed when possible). Only emit if zh‑Hans is planned for the collection.

8) Media scanning, provenance & storage
8.1 Media scan → mime/bytes/hashes (YELLOW inputs feeding GREEN checks)
	•	Local scan to CSV (one‑liners):
	◦	exiftool -r -csv -FileName -MIMEType -FileSize# . > media_scan.csv
	•	Columns to import:
	◦	image_mime / animation_mime
	◦	image_bytes / animation_bytes
	◦	(optional) image_sha256 / animation_sha256 (from Python helper)
	•	Script uses these for: file extension, size QA, and checksum_ok.
8.2 Provenance commitment - What should this say and where is the input?
	•	Maintain a per‑collection ordered list of media (by slug).
	•	Script computes provenance_block_sha = SHA‑256 of concatenated cids or hashes.
	•	Store commitment in sheet; optionally in contract (if supported).
8.3 Storage & delivery fields (BLUE/YELLOW)
	•	Redundant pinning status: pin_status- Can you explain this to me please, what do i input?
	•	Gateway QA: gateway_qa (OK/Fail + note) What does on the note? 
	•	Derivative folders: /master, /marketplace_optimized, /thumbs
	•	Video extras: aspect_ratio, fps, duration_ms - Is this needed, how important, this added as green columns?

9) Reveal, editions & redeem/burn - Can you explain this to me please, what do i input and where?
	•	Reveal: reveal_mode, reveal_date, pre_reveal_cid
	•	Edition math: edition_size, token_range, edition_label
	•	Redeem/burn: burnable, redeem_target, redemption_window, claims_remaining

10) Visual taxonomy hygiene -  Can you explain this to me please, what do i input and where?
	•	Normalize & case‑lock: Series, Character, Character_Variant, Frame_Style, Colorway, Finish (e.g., Round Canvas), Series_Arc, Symbols (multi‑trait emit).
	•	Optional: Rarity_Tier (for display sorting; not for RNG rarity).

11) QA & compliance checklist (run per batch) -Run per batch? Where is it indictaed that it was successfully passed QA?
	•	IPFS CIDs resolve on ≥2 public gateways.
	•	All JSON validates and renders on OpenSea test page. - What if we are using a different platform?
	•	Thumbnails load; video autoplay OK; file sizes under platform limits.
	•	Royalties recognized by OS/Blur spot‑checks.
	•	Unlockables gated correctly (test with burner wallet).
	•	Per‑token page loads; license link works; alt text present.
	•	Backup sheet, media, and contract ABIs in versioned storage.

12) Update‑script scope (what RUN_updateAll_now does)
Header‑safe: locate columns by name each run.
	1	Normalize & guards
	•	Tier normalizer (5 labels only), clear #N/A/#REF! in non‑EN language cols.
	•	Slug uniqueness (append -token_id only when needed).
	•	HEX check (background_hex blank or #RRGGBB).
	•	token_range empty or numeric range.
	2	Derivations (GREEN)
	•	slug, hook_line, alt_text_*_auto
	•	attributes_json (ordered traits, multi‑emit Symbols)
	•	category_id/subcategory_id via lookup tab
	•	image_url / animation_url; preview_thumb formula injection (header‑safe)
	•	media/json filenames; external_url_built; edition_string
	•	len_* counters & status_* lights; missing_fields / warnings; json_will_emit
	•	sot_status
	•	checksum_ok (if *_bytes/sha present)
	•	(optional) provenance_block_sha per collection
	3	SoT linking (YELLOW fetch)
	•	caption_long_en / research_notes / sources_bibliography / sot_link
	4	No auto‑recoloring (respect GHM Profile). Optional helper RUN_colorize_once() if you need it, but not in the main update.

13) Minimal GAS patch (header‑safe) — drop‑in skeleton
(Paste into your existing Apps Script project; wire this to your button.)

function RUN_updateAll_now() {
const ss = SpreadsheetApp.getActive();
const sh = ss.getActiveSheet();
const H = headerMap_(sh); // {header: colIndex}
const rng = (h) => sh.getRange(2, H[h], sh.getLastRow()-1, 1);

// 1) Normalize & guards
normalizeTier_(sh, H);
clearLangErrors_(sh, H, ['title_', 'description_', 'alt_text_']);
enforceHex_(sh, H, 'background_hex');
enforceTokenRange_(sh, H, 'token_range');

// 2) Derivations
fillSlug_(sh, H);
fillHookLine_(sh, H);
fillAltText_(sh, H);
fillTraitJson_(sh, H);
fillTaxonomyIds_(sh, H);
fillMediaUrls_(sh, H); // image_url / animation_url
fillPreviewThumb_(sh, H); // sets IMAGE() formula header‑safe
fillFilenames_(sh, H); // media_filename / json_filename
fillExternalUrl_(sh, H);
fillEditionString_(sh, H);
fillLengthsAndStatus_(sh, H);
fillJsonGateAndWarnings_(sh, H);
fillSoTStatus_(sh, H);
fillChecksumOk_(sh, H);

// 3) SoT pulls
pullSoT_(sh, H); // caption_long_en, research_notes, sources_bibliography, sot_link
}

function headerMap_(sh){
const values = sh.getRange(1,1,1,sh.getLastColumn()).getValues()[0];
const map = {};
values.forEach((h,i)=>map[String(h).trim()]=i+1);
return map;
}

function fillPreviewThumb_(sh, H){
const r = sh.getRange(2, H['preview_thumb'], sh.getLastRow()-1, 1);
const imgCol = columnLetter_(H['image_url']);
// Header‑safe IMAGE formula referencing same row image_url
for (let i=2;i<=sh.getLastRow();i++){
sh.getRange(i, H['preview_thumb']).setFormula(`=IF(${imgCol}${i}<>"", IMAGE(${imgCol}${i}), "")`);
}
}

function columnLetter_(col){
let temp = '', letter = '';
while (col>0){
temp = (col-1)%26;
letter = String.fromCharCode(temp+65) + letter;
col = (col - temp - 1)/26;
}
return letter;
}


Note: The helper functions (e.g., fillSlug_, fillAltText_, fillTraitJson_) are straightforward row mappers; keep them pure, reading only the headers they need and writing back a single column each.

14) Column index — additions since last pass - Can you tell me the recommended colour?
Manifest (new or promoted):
	•	alt_text_zh‑Hans_auto (only if zh planned)
	•	edition_label
	•	burnable, redeem_target, redemption_window, claims_remaining
	•	background_hex (alias "background_color" → standardized to background_hex)
	•	image_bytes, animation_bytes, image_sha256, animation_sha256
	•	provenance_block_sha (optional)
	•	print_eligible, physical_redeemable, print_eligible_inferred
	•	thumb_url (if using external thumbs)
	•	Video extras: aspect_ratio, fps, duration_ms
Control (new): - Can you tell me the recommended colour?
	•	freeze_policy, freeze_date
	•	est_gas_mint, est_gas_batch
	•	license_type, terms_url, physical_terms_url, moral_rights_statement
	•	primary_mint_platform, marketplace_targets, verification_status
	•	meta_title, meta_description, og_image, canonical_domain_or_ENS
	•	batch_release_plan_url, press_kit_url, support_email

If you want, I can now generate the tiny helper functions (fillSlug_, fillAltText_, fillTraitJson_, etc.) to paste straight into your Apps Script  - YES please

15) Implementation order of operations
	1	Add headers above (Manifest + Control + SoT tab). Do not recolor automatically.
	2	Paste the GAS patch and connect to your existing button.
	3	Import a media_scan.csv after local exiftool run; map FileName to your image_filename/animation_filename to fill mime/bytes.
	4	Run RUN_updateAll_now → check missing_fields/warnings.
	5	Validate JSON on test marketplace; spot‑check royalties & thumbnails.
	6	Decide freeze policy & reveal plan; populate Control → re‑run update.

16) Appendices
A) One‑liner media scan
exiftool -r -csv -FileName -MIMEType -FileSize# . > media_scan.csv
B) Alt‑text field map  
	•	Character → {Character}
	•	Variant → ({Character_Variant}) when present
	•	Style/Finish → {Style} / {Finish}
	•	Frame → with {Frame_Style} (optional)
	•	Colorway → {Colorway} palette
	•	Meaning → {meaning_line} (short)
	•	Tier → {Tier} edition
C) Trait emission order (attributes_json)
Pantheon → Series/Deity Collection → Character → Variant/Location → Myth Scene → Style/Format → Tier → Edition Size → Symbols → Colorway → Frame

Done. This spec is ready to apply to your current Manifest/Control sheets without breaking header positions, and it respects the GHM color policy (no auto‑repainting).
Got it — I merged everything into a single, ready-to-apply spec and put it in the canvas: “Column Autos & Dynamic Content — merged spec (v2025-10-17)”.
What’s new vs our last pass:
	•	Pulled in the media scan → mime/bytes/hashes workflow (exiftool one-liner + optional Python for SHA-256) and bilingual field handling from your memo, then wired those into GREEN checks (checksum_ok, file-extension logic, etc.). 
	•	Added background_hex, provenance commitment (per-collection SHA), reveal plan fields, print/physical flags, unlockables delivery + gating, storage/pinning & video extras, marketplace/SEO ops, and a beefed-up QA checklist—all mapped cleanly to Manifest/Control/SoT. 
I also included:
	•	A header-safe GAS patch (drop-in skeleton) that extends your RUN_updateAll_now to fill image/animation URLs, inject preview thumbnails, link SoT long copy, run validators, and set JSON gates — while not auto-repainting colors (per your GHM profile preference).
	•	Exact column adds (Manifest, Control, SoT), length budgets, alt-text templates, and the trait-emission order you’re using.
If you want, I can now generate the tiny helper functions (fillSlug_, fillAltText_, fillTraitJson_, etc.) to paste straight into your Apps Script — just say the word and I’ll drop them in matching your headers.



