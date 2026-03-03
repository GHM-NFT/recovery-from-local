1) Column colours (what they mean)
	•	Red = Manual You (or your team) must fill/edit these. Examples: Title/Name, Pantheon, Frame, Pallette, Format/Medium, Stylisation, License_URL, price_native, currency, chain, external_url, description, Contract/Contract address.
	•	Blue-lilac = Manual (once/setup) You set these once per row or per drop, then rarely touch. Examples: filenames, paths, CIDs, unlockable ZIP fields (image_filename, animation_filename, unlockable_zip_*), archival refs (Masters, Previews, etc.).
	•	Yellow = Auto-if-available These can be derived from other fields or a dictionary, but you can override. Examples: Series, Character, Character_Variant, Frame_Style, Colorway, Edition_Type.
	•	Green = Auto Script/formula-populated; you generally shouldn’t overwrite. Examples: name_final, slug, alt_text_en, attributes_json, image_mime, image_bytes, animation_mime, animation_bytes, operator_filter, operator_policy_note, schema_version, category_id, subcategory_id, taxonomy_tags.
(If any column you expect to be coloured isn’t, it’s usually a header spelling/alias—easy to add to the alias map.)
Colors (same across tabs)
	•	RED (Manual): you curate by hand
	◦	name_final, background_hex, collection, Series, Character, Character_Variant, Style, Tier, display_order
	◦	English & Story: title_en, description_en, alt_text_en, myth_scene, meaning_line, caption_300, symbols
	◦	Optional curation: taxonomy_tags, hashtags_social, unlockable_notes
	•	BLUE (Setup / Required refs): usually one-time or admin inputs
	◦	Admin/IDs: standard, operator_filter, chain, contract, contract_address, contract_factory, token_id, edition_size, edition_number
	◦	Media refs: cid_Poster_Image, image_cid, animation_cid, external_url
	◦	Rights/Commerce: license_url, license_attr_value, creator, contributors, copyright_year, credit_line, price_native, currency, royalty_bps, fee_recipient, listing_status, marketplace
	◦	Unlockables (canonical): unlockable_flag, unlockable_type, unlockable_cid
	◦	Taxonomy IDs: category_id, subcategory_id
	◦	slug (blue because it’s a naming artifact, but we auto-suggest a candidate elsewhere)
	•	YELLOW (Auto-if-available / links out):
	◦	export_batch_id, last_qc_at, last_modified (admin timeline)
	◦	opensea_permalink, json_preview_url
	•	GREEN (Auto): computed helpers you shouldn’t edit
	◦	image_url, animation_url, external_url_built, attributes_json, edition_string, json_will_emit, missing_fields, warnings, checksum_ok
	◦	Length + status: len_* and status_*
	◦	Builders: slug_candidate, media_filename, json_filename
	◦	Media tech: image_mime, image_bytes, image_width, image_height, image_checksum, animation_mime, animation_bytes, animation_width, animation_height, animation_duration_s

TL;DR — What you must fill vs optional
	•	Must (for metadata export): RED: name_final, Series, Character (if applicable), Character_Variant (if used), Style, Tier, title_en, description_en, alt_text_en, myth_scene, meaning_line, caption_300, symbols, edition_size, background_hex. BLUE: image_cid (and cid_Poster_Image), license_url (or at least choose your license stance), price_native/currency if listing, unlockable_* (only if your item has unlockables).
	•	Optional / record only (hidden by Curator View): Ops assets (Masters/Previews/Renders/Thumbnails and cid_* variants), file paths, packaging zips, ops notes, schema version.


2) Column titles (what they’re for)
Very briefly, grouped by purpose:
ID & naming
	•	token_id – Token number (on-chain ID).
	•	Title/Name – Human title for the piece.
	•	name_final – Final on-chain name (auto from components unless overridden).
	•	slug – URL-safe identifier (auto).
Lore & visual
	•	Pantheon, Meaning/Story, Tier, Stylisation, Frame, Pallette, Format/Medium, Frame_Style, Colorway, Edition_Type, Medium – Curation/trait/lore descriptors shown in attributes/UI.
Licensing
	•	License_URL, license – Link and/or label for license.
Media (human-entered)
	•	image_filename, animation_filename, background_color, unlockable_zip_*, unlockable_notes – Files and unlockables you provide.
Media (auto)
	•	image_mime, image_bytes, animation_mime, animation_bytes – Filled from your media scan merge scripts.
Paths & CIDs (setup / archive)
	•	collection_path, image_path, json_path, unlockable_path, image_cid, json_cid, cid_* – Storage references (IPFS/local). Blue “manual once”.
Taxonomy / indexing
	•	Series, Character, Character_Variant – Canonical identifiers.
	•	category_id, subcategory_id, taxonomy_tags – Auto from Taxonomy_Mapping (when present).
Marketplace / chain
	•	standard – 721 or 1155.
	•	contract_factory – Where/how you deploy (OpenSea / Manifold / Zora / Custom).
	•	Contract / Contract address – The on-chain collection address (0x…).
	•	price_native, currency, chain, external_url – Pricing, network, and landing.
Language / accessibility
	•	title_en, description_en, alt_text_en
	•	title_zh-Hans, description_zh-Hans, alt_text_zh-Hans (see #5 for other languages)
JSON & schema
	•	attributes_json – If present, used verbatim for traits.
	•	schema_version – Version pin for downstream exporters.
	•	Add, FFFFFF – your custom helper columns (OK to keep/rename as needed).

3) GHM menu (Control / View / Color / Maintenance)
	•	Control
	◦	Setup Control – Creates/refreshes the Control tab with defaults & lists.
	◦	Apply Control to Tabs (safe) – Applies dropdowns & defaults (standard, contract_factory, operator stance, license/background) to all collection tabs. Current: up to date.
	•	Views
	◦	Compact (Active / All) – Builds compact sheets showing only the selected working columns (or you can use “Show Only Columns” to hide in-place).
	◦	Clean Compact Aliases – Removes old duplicate compact tabs. Current: up to date. You also have “Show/Unhide Only Columns” to do in-place compact on collection tabs.
	•	Color
	◦	Repaint This Tab (columns) – Re-applies the red/blue/yellow/green roles based on header names. Current: up to date (add aliases if a header is uncoloured).
	•	Maintenance
	◦	Sanitize Text Columns – Clears invalid validations on free-text fields (fixes “BU2 violates validation” type errors).
	◦	Fix Standard/Contract – Rebuilds those dropdowns everywhere.
	◦	Dedup contract_factory – Merges/removes duplicate headers.
	◦	Rebuild Control Report – Writes GHM_CONTROL_APPLY_REPORT with OK/ERROR per tab. Current: up to date.
Nothing is “out of date” now; if you want extra actions (e.g., “Add Language Columns”), we can add them.

4) Bottom tabs (what/why; auto vs manual; safe to delete?)
	•	GHM_BU_AUDIT – Batch/update audit scratch (automation diagnostics). Auto. Keep.
	•	Marketplace_Preview – Pretty preview of how a listing will look. Auto from collection tabs. Keep (optional, but useful).
	•	Metadata_QC – Checks required fields, lengths, JSON-readiness. Auto. Keep.
	•	GHM_DEBUG – Diagnostics dump (matches, headers, scripts). Auto. Keep while building; can hide later.
	•	GHM_CONTROL_APPLY_REPORT – Results of Control → Apply (errors like invalid dropdowns). Auto. Keep.
	•	Taxonomy_Categories – Your reference/category list (manual dictionary). Manual / dictionary. Keep if you use taxonomy.
	•	Control – Global defaults (operator policy, defaults, etc.). Manual. Must keep.
	•	GHM_SELFTEST_REPORT – Quick self-test results. Auto. Keep (or hide).
	•	ERC1155 – Editions – Helper for edition planning. Manual table. Keep if using 1155; else hide.
	•	All – All rows from all collections combined. Auto. Keep.
	•	Collections_Index – Index of collections. Auto. Keep.
	•	Characters_Index – Index of characters. Auto. Keep.
	•	Series_Character_Matrix – Cross table (series × characters). Auto. Keep.
	•	Traits_Dictionary – Central traits dictionary (names/allowed values). Manual dictionary; feeds auto-if columns. Keep.

