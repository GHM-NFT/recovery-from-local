You’ve done brilliantly getting the sheet clean. Here’s a crisp, practical checklist of autos to add/keep, plus the SoT (Source-of-Truth) linking for long copy. I’ll show: Column → Color → What it does → Uses.
GREEN (computed/derived, run by your “Update All” button)
	•	slug → Green → Creates a unique, lowercase, hyphen slug. → Uses Series, Character, Character_Variant, myth_scene, Style (+ token_id to dedupe).
	•	hook_line → Green → Short one-liner for cards. → name_final, Character, Character_Variant, Style, Tier, myth_scene.
	•	alt_text_en_auto → Green → Clean, consistent alt text (metadata-based). → Character, Character_Variant, Style, Colorway, Frame, meaning_line.
	•	attributes_json → Green → Emits the traits JSON your metadata needs. → Series, Character, Character_Variant, Style, Frame, Colorway, Tier, Symbols (+ optional extras later).
	•	category_id, subcategory_id → Green → Auto-map taxonomy. → Looks up Series (or a map tab) → IDs.
	•	image_url → Green → Builds a gateway URL from image_cid for previews/QA. → image_cid.
	•	animation_url (if used) → Green → Gateway URL for video. → animation_cid.
	•	preview_thumb → Green → In-cell thumbnail (e.g., 72×72) for quick visual context. → image_url (IMAGE formula).
	•	media_filename → Green → File stem for export. → slug + file extension from image_mime.
	•	json_filename → Green → JSON filename for export. → slug + “.json”.
	•	external_url_built → Green → Your site’s per-token URL. → Base site URL (constant) + slug.
	•	edition_string → Green → Nice display string (e.g., “Edition of 10”). → edition_size (you’re storing total only).
	•	json_will_emit → Green → TRUE/FALSE gate: do we have everything required to build JSON? → Checks required reds/blues present.
	•	missing_fields / warnings → Green → Human-readable issues (missing fields, too-long copy, bad hex). → Validates key reds/blues + lengths.
	•	len_ / status_** → Green → Length counters + traffic-light status for: name_final, Series, Character, Character_Variant, myth_scene, Style, Tier, edition_size, slug, description_en, caption_300. → Same as above.
	•	checksum_ok (optional) → Green → Flags zero/empty bytes or mismatches. → image_bytes (and checksum if you store it).
YELLOW (auto-if-available / linked from SoT)
	•	caption_long_en (or caption_1200) → Yellow → Pulls longer copy from SoT when present. → XLOOKUP by slug from a SoT tab.
	•	research_notes → Yellow → Internal notes from SoT. → XLOOKUP by slug.
	•	sources_bibliography → Yellow → Links/citations from SoT. → XLOOKUP by slug.
	•	opensea_permalink, json_preview_url → Yellow → Filled post-upload. → Written by ops step later.
	•	json_path, json_cid → Yellow → Written by packager/export. → From export step.
	•	unlockable_zip_ (filename/url/bytes/sha256)* → Yellow → Written by packager. → From export step.
BLUE (setup/ops you already have, keep)
	•	license_url, license_attr_value (done), price_native, currency, royalty_bps, fee_recipient, operator_filter, chain, contract_address, contract_factory, standard, image_filename, animation_filename, cid_Poster_Image, external_url (collection landing page), token_range/collection_path (if you use them).
PREVIEW IMAGE — yes, include
	•	image_url (Green) from image_cid.
	•	preview_thumb (Green) = small IMAGE() from image_url. (We’ll write the IMAGE formula via the Update button so it’s header-safe.)
LONG COPY — link it to a SoT (clean & legible)
Create a tab (e.g., GHM_SoT) with one row per work, keyed by slug, with columns:
	•	slug, caption_long_en, research_notes, sources_bibliography, (optional) sot_doc_url. Then in the manifest:
	•	caption_long_en (Yellow) ← XLOOKUP from GHM_SoT!caption_long_en by slug.
	•	research_notes (Yellow) ← XLOOKUP by slug.
	•	sources_bibliography (Yellow) ← XLOOKUP by slug.
	•	sot_status (Green) ← TRUE/FALSE if slug found (helps you spot missing SoT rows).
	•	sot_link (Blue) ← HYPERLINK to sot_doc_url if you prefer drafting in Docs.
CLEANUP you mentioned (we’ll wire into “Update All”)
	•	Language errors: clear #N/A, #REF!, etc. in non-EN cols (optionally fallback to EN).
	•	Tier normaliser: enforce the 5 labels.
	•	Slug uniqueness: append -token_id if a duplicate slug would occur.
	•	HEX check: ensure background_hex is blank or #RRGGBB.
	•	token_range: ensure empty or 1-10 / 5 (no JSON sneaking in).

How we’ll run it
	•	Keep formulas to a minimum (thumbnails only).
	•	Everything else is set by your “Update All” button so it’s header-safe and won’t break if columns move.
	•	We’ll extend your button to also (a) set preview thumbnails, (b) fill SoT lookups (or leave blank if missing), and (c) run the language clean.
If you want, I’ll next provide a tiny update to your existing RUN_updateAll_now that adds:
	•	image_url, animation_url, preview_thumb,
	•	SoT lookups for caption_long_en / research_notes / sources_bibliography / sot_status / sot_link,
	•	the length/status checks.
